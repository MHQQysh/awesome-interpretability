# 131. The Troubling Emergence of Hallucination in Large Language Models

- **Authors:** Vipula Rawte et al.
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.155
- **Tags:** Hallucination, Factuality, Benchmark, Analysis, Mitigation

## Introduction

论文从 ChatGPT、Bard 和真实使用事故出发，指出“会生成流畅文本”与“生成事实”不是同一能力。已有工作通常按 intrinsic / extrinsic 或按任务零散讨论幻觉，缺少一个跨任务、能比较不同 LLM 的细粒度分类和统一风险分数。作者还强调，幻觉未必全部有害：有些摘要中的补充信息可能是有益的，因此需要同时描述方向、类型和严重程度，而不是只做二元 hallucination / no-hallucination 判断。

核心问题是：给定事实正确和事实错误两类输入，15 个模型分别会产生什么样的偏离？这些偏离能否被人工标注为稳定类别，并汇总为一个可比较的 Hallucination Vulnerability Index（HVI）？

## Method / Framework

1. **两种方向。** Factual Mirage（FM）表示对事实正确 prompt 产生偏离；Silver Lining（SL）表示对事实错误 prompt 进行迎合或添加看似合理但未经支持的信息。两种方向再拆为 intrinsic 和 extrinsic。
2. **三种严重度。** mild、moderate、alarming，按新信息与 prompt 主题的偏离幅度递增。
3. **六种类型。** Acronym Ambiguity、Numeric Nuisance、Generated Golem、Virtual Voice、Geographic Erratum、Time Wrap。它们分别覆盖缩写歧义、数字错误、编造实体或事件、错误人物声音、地点错误和时间错误。
4. **HILT 数据集。** 用 NYTimes tweets 作为事实正确 prompt、Politifact 作为事实错误 prompt，让 15 个模型生成总计 75,000 个片段；每句由 4 位 Amazon Mechanical Turk 标注者标注，使用 MACE 聚合并估计标注可靠性。
5. **HVI。** 对模型 x，统计总句数 U、各类型幻觉数 N(x)，同时区分 FM 与 SL 的比例，并用 δ1、δ2 对不同方向和模型排名做阻尼，再缩放到 0 到 100。HVI 的目的不是替代下游任务 accuracy，而是衡量“模型本身产生幻觉的倾向”。
6. **两种缓解。** ENTROPYBB 在黑盒条件下找高熵词，并用低 HVI 模型替换；FACTUALITYGB 用 Google Search 获取前 20 个文档，再用 RoBERTa-large textual entailment 将句子分为 support、refute、not enough information，后两类交给人检查。

## Baselines / Comparisons

这不是一个单一任务的 SOTA 比赛，主要比较 15 个模型的 HVI 光谱，包括 GPT-3、StableLM、GPT-2、Vicuna、MPT、LLaMA、GPT-3.5、Dolly、OPT、GPT-4、Alpaca、BLOOM、T0、XLNet 和 T5。它也把 HVI 与 accuracy、precision、recall、F1 区分开：后者依赖具体任务，HVI 试图给出跨 NLG 场景的风险尺度。缓解部分则比较不同检测器与替换器组合，报告 HVI 下降而不是只报告文本相似度。

## Experiments / Findings

- HVI 排名中 GPT-3 约 90、StableLM 82、GPT-2 70、Vicuna 62、MPT 59、LLaMA 57、GPT-3.5 53、Dolly 49、OPT 48、GPT-4 47，T5 32；该排名说明模型规模本身不能单独解释幻觉风险。
- 不使用 RLHF 的大模型更容易同时出现 FM 和 SL。小模型中 Generated Golem、Virtual Voice、Geographic Erratum 较少；随着规模增长，复杂的 Time Wrap 和 Geographic Erratum 更明显，Virtual Voice 在 GPT-3.5 到 GPT-4 间显著增加。
- 论文没有把“更大”简单等同于“更差”：训练数据质量、是否显式训练事实、生成时过度自信等因素也会影响 HVI。
- 对 GPT-3 的缓解实验中，ALBERT-large-v2 更适合识别高熵词，DistilRoBERTa-base 更适合替换；表 2 的最大 HVI 降幅为 10.66。把连续高熵词作为一个 span 一起 mask，比逐词替换更适合 Generated Golem 和 Acronym Ambiguity。
- FACTUALITYGB 在句子层面得到约 26% 的 alert rate，说明自动检索和 entailment 可以先缩小人工复核范围，但不能把 alert 直接当成事实错误。

## Ablation / Error Analysis

HVI 的 δ1、δ2 会影响 FM 与 SL 的相对权重，因此作者先以 0 阻尼计算初始 HVI，再根据均值和标准差重新计算、排序和缩放。缓解分析还比较了 entropy detector 和 textual-entailment verifier，指出前者擅长较简单的缩写和数字问题，后者覆盖需要外部证据的事实性问题。附录对 2,000 个缓解样本做了 6 人人工判断，用于确认替换后的幻觉是否真正减轻，而不是只降低模型分数。

## Limitations

分类体系是人为设计的，可能遗漏 name-nationality 等新型幻觉或类别交叉。15 个模型只是 2023 年的一个快照，模型和版本不断变化，需要重新评估。HVI 依赖人工标注和模型选择，不能自动保证每个类别的事实判断正确；FACTUALITYGB 还受搜索结果质量和 entailment 模型偏差影响。作者也承认用“高熵词替换”可能损害语义或流畅性，且 FACTUALITYGB 仍需要人工复核。

## 两句话总结

论文把 LLM 幻觉从一个模糊的二元现象拆成方向、类型和严重度，并用 75,000 条 HILT 数据和 HVI 让不同模型的风险可以比较。它进一步展示了高熵替换和检索加蕴含判断两条缓解路径，但 HVI 更适合作为风险筛查与研究基准，而不是事实性的最终证明。
