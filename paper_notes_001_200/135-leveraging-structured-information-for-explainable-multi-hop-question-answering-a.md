# 135. Leveraging Structured Information for Explainable Multi-hop Question Answering and Reasoning

- **Authors:** Ruosen Li, Xinya Du
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.452
- **Tags:** Explainable QA, Multi-hop Reasoning, Semantic Graph, Faithfulness

## Introduction

Multi-hop QA 需要把分散在多段文档中的实体和关系组合起来。CoT 让 LLM 输出自然语言 reasoning chain，提升了可读性和一定的准确率，但它仍是端到端生成：模型可能多跳、跳错、编造不存在的中间事实，生成的解释也可能只是事后合理化。传统 graph-based QA 的图通常只记录实体连接，不保留细粒度语义关系。

论文提出的问题是：能否先从文档抽取有关系标签的 semantic graph，再把它作为 prompt 中的显式结构，约束 LLM 的推理路径，并让解释直接落到输入文本？

## Method / Framework

方法叫 SG prompting，流程为：

1. 用 LLM 做信息抽取，把文档转成三元组，如（Anne, daughter of, Jean）、（Jean, youngest child of, Robert）。
2. 将多个文档中的实体、关系和事件组成 semantic graph；边带有语义关系，而不只是无标签连接。
3. 对图做 sequentialization，把三元组拼成 prompt；SG-One 只保留与问题相关的图，SG-Multi 使用多个候选文档的结构，G-Full 则提供更密集的全实体关系。
4. LLM 在结构化输入上生成答案与 reasoning chain。方法不需要针对 QA 任务额外 fine-tuning，直接使用 GPT-3.5 的 in-context learning。

解释由两部分构成：结构图本身是从上下文抽出的 grounded explanation，生成的 chain 则是模型在图上导航后的自然语言表达。作者因此同时评价答案准确率、chain faithfulness 和人类偏好。

## Baselines / Comparisons

CoT setting 下比较 Base prompt、G-Full、SG-One、SG-Multi；few-shot setting 也比较 Base、G-Full、SG-Multi。数据集是 HotpotQA 和 2WikiMultiHopQA，每题提供 10 个候选段落。指标包括 EM、F1、precision、recall；reasoning chain 另外比较人工判断、ROUGE-1/2/L 和 BERTScore。

## Experiments / Findings

- HotpotQA 上，加入结构图总体提高四项 QA 指标，SG-Multi 的 recall 比 base prompt 提高约 4%。G-Full 有时反而降低 recall，因为全实体关系带入 spurious information，图太长后还会触发 context limit。
- 2WikiMultiHopQA 上，加入图后 EM、F1、precision 平均提升约 9%、6%、6%。作者认为 recall 更适合生成式 QA，因为模型生成的正确答案可能比 gold span 更长，precision / EM 会惩罚额外但合理的信息。
- 100 个 2WikiMultiHopQA 样本的人工 chain 评测中，Base、G-Full、SG-Multi 的 human faithfulness / quality 分别为 0.81、0.87、0.88；SG-Multi 的 ROUGE-1/2/L 为 0.689/0.543/0.628，略高于其它 prompt。
- 结构图帮助模型在正确答案处停止，减少 over-reasoning；同时让模型更容易知道某个关系不在证据中，而不是继续编造。
- 人工 pairwise 评测显示，参与者明显偏好 SG-based graph explanation，而不是纯 NLG chain 或 saliency explanation，因为图中的节点和边能直接回溯到输入。

## Ablation / Error Analysis

SG-One 与 SG-Multi 的差别体现了图大小和相关性的重要性：更小、更聚焦的图通常比 G-Full 更 faithful。ROUGE 与人工 faithfulness 的 Spearman 相关约 0.32，BERTScore 约 0.24，说明自动指标只能提供中等程度的近似。错误主要来自外部常识不足、图抽取不完整、隐喻或 paraphrase 关系、以及约 5% 的不可回答问题；还有约 3/100 样本因为 LLM 输出长度限制导致图缺信息。

## Limitations

额外的 IE 抽取步骤增加推理时间和 API 成本。图质量依赖 LLM 的抽取能力和 context window，漏掉一个关键三元组会同时损害答案和解释。把图序列化为文本仍可能超过上下文窗口，也没有真正执行符号推理；系统只能约束 prompt，不能保证生成 chain 每一步都忠实于图。

## 两句话总结

论文用带语义关系的结构图把 multi-hop QA 的推理从纯文本生成转成“从证据图中导航”，并让图本身成为 grounded explanation。HotpotQA、2WikiMultiHopQA 和人工评测显示 SG-Multi 提高了答案与 chain faithfulness，但图抽取错误、长度和额外成本仍是主要瓶颈。
