# 375. LLMs-as-Judges: A Comprehensive Survey on LLM-based Evaluation Methods

- **Authors:** Haitao Li, Qian Dong, Junjie Chen, Huixue Su, Yujia Zhou, Qingyao Ai, Ziyi Ye, Yiqun Liu
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2412.05579
- **Tags:** `survey` `LLM-as-a-judge` `evaluation` `bias` `meta-evaluation`

## Introduction
LLM 已从被评测对象变成评测者：在开放式生成、对话、代码、推理和多模态任务中，人工逐条打分太贵，而 exact-match 或固定 reference 又无法覆盖多种合理答案。因此研究者让一个 LLM 对 response 做 pointwise scoring、pairwise comparison、ranking 或提供 rationale。

这篇 survey 关心的不是“哪个 judge 分数最高”，而是如何系统理解 judge 的功能、方法、应用和可靠性。它把领域拆成五个视角：Functionality、Methodology、Applications、Meta-evaluation、Limitations，并将 judge 的输出视为一种带有输入、比较对象、评价标准、聚合方式和解释的评测函数。

## Method / Framework
作为综述，论文没有提出一个新的训练模型，而是建立统一分类框架：

1. **Functionality:** pointwise evaluation 给单个答案评分或判断；pairwise evaluation 比较 A/B；ranking 同时排序多个答案；judge 还可能输出自然语言 rationale、错误类型或结构化 rubric。
2. **Methodology:** single-LLM judge、multi-LLM judge 和 human-LLM collaborative judge。方法维度包括 zero/few-shot prompting、rubric decomposition、chain-of-thought、reference-based/reference-free 评价、self-consistency、多个 judge 的聚合以及校准。
3. **Applications:** 通用对话和 instruction following、摘要/翻译/问答、代码、数学/推理、对齐/安全、检索增强和多模态生成等。
4. **Meta-evaluation:** 用人类标签或任务指标评价 judge 与人类的一致性、偏好排序能力、稳定性、解释质量和跨域泛化。
5. **Limitations:** position bias、verbosity/style bias、self-preference、data contamination、judge hallucination、prompt sensitivity、低可复现性和成本/延迟。

## Baselines / Comparisons
综述将传统 automatic metrics、人工评价、reward model/classifier 和 LLM judge 放在同一评价链中。BLEU/ROUGE 等 reference-based 指标便宜稳定但难处理开放式语义；人工评价更接近目标却昂贵且有主观差异；专门 reward model 成本较低但需要标注和训练；LLM judge 具有强语言理解和可解释 rationale，却继承基础模型偏差。

在 judge 内部，常见比较是 single judge vs multi-judge、pointwise vs pairwise、zero-shot vs few-shot/CoT、有无 reference、单次输出 vs repeated/self-consistency。survey 强调不能只比较总体相关系数：不同功能对应不同 ground truth，pairwise agreement、ranking correlation、calibration 和 judge stability 需要分别测量。

## Experiments / Findings
论文是方法综述而非统一 benchmark，因此没有一张可以横向宣称“某模型最优”的主结果表。它汇总的领域发展流程是：早期用规则和 reference metrics；开放式生成促使人工比较；强 LLM 出现后形成 LLM-as-a-judge；随后研究从单一总分转向 rubric、rationale、multi-agent/jury、校准和 bias 诊断。

综合 findings 有三点：第一，pairwise 比绝对分数更容易让 judge 做稳定选择，但会丢失差距大小和 transitivity；第二，CoT/rationale 往往提升可审计性，却不自动保证 rationale faithful，甚至可能让模型更有说服力地犯错；第三，judge 与人类的相关性高度依赖任务、prompt、候选答案风格和参考信息，不能把一个数据集上的 agreement 当成普适可靠性。

## Ablation / Error Analysis
survey 将常见错误模式本身视为需要做的“消融”：交换 A/B 位置检查 position bias；控制长度检查 verbosity bias；让 judge 评价自己的输出检查 self-preference；改变 prompt、temperature 和 rubric 检查 prompt sensitivity；重复调用和更换 judge 检查 variance；加入 adversarial 或低质量但流畅的答案检查 robustness。

另一个重要结论是 explanation 评价不能只看语言流畅度。应该分别检查 rationale 是否覆盖评分标准、是否引用了真实 response 证据、去掉 rationale 后分数是否改变、以及 rationale 的错误是否与最终分数一致。综述没有把某个诊断流程包装成已解决方案，而是明确指出这些 meta-evaluation 仍缺乏统一协议。

## Limitations
survey 的分类会随着新模型、judge agent 和多模态评测快速变化；不同论文使用不同 judge、prompt、样本和人类标签，汇总结果很难严格 apples-to-apples。大量研究把 GPT-4 等闭源模型当 judge，版本漂移和调用成本影响复现。更根本的限制是“人类一致”不等于“正确”：人类标签有噪声，开放式质量也可能没有唯一标准，judge 的 rationale 也可能只是事后合理化。

## 两句话总结
这篇 survey 把 LLM-as-a-judge 组织为功能、方法、应用、meta-evaluation 和局限五个维度，梳理了从单一打分到 pairwise、rubric、jury、校准与偏差诊断的发展流程。它的核心警告是 judge 分数和 rationale 都必须经过位置、长度、自偏好、稳定性和人类一致性测试，不能把一个相关系数或一段流畅解释当成可靠评测。

## Evidence note
已读取 arXiv 2412.05579 v2 本地 PDF 的摘要、分类框架、方法、meta-evaluation 与 limitations 章节；本文为 survey，没有虚构统一实验数字。
