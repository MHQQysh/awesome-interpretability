# 048. Properties and Challenges of LLM-Generated Explanations

- **作者 / venue**：HCINLP 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.hcinlp-1.2/)
- **类型**：研究 LLM explanation 的属性、适用范围和人类评价

## Introduction

LLM 可以为大量问题生成自然语言 explanation，但解释并不总是忠实或有用。论文关注 explanation 的属性：selectivity、illustrativeness、合理性、长度、语气和是否真的回答了“为什么”。

## Method / evaluation

- 构造多种 instruction/task，要求模型回答后解释。
- 对解释做人工问卷评价，区分“是否包含解释”和“人类是否会用它来 justify reasoning”。
- 分析不同问题类型、事实问答、数学问题和主观任务的 explanation pattern。
- 研究 prompt 是否要求解释、解释是否和答案同时生成对 explanation properties 的影响。

## Baseline 与对比

- 无 explanation 的 direct answer。
- 要求简短理由、详细 rationale、friend-style justification 等 prompt。
- GPT-4 等模型及不同任务类别。
- 人工 annotator 对“像不像解释”和“是否真的能 justify”两种判断。

## Findings

- LLM 生成 explanation 会模仿训练数据中的人类表达方式，常表现出 selectivity 和 illustrative examples。
- 模型可能自信地解释错误答案或模糊主观判断，这会提高用户的错误信任。
- explanation 的存在性、可读性和忠实性不是同一个维度；一个回答即使能生成长理由，也不代表模型使用了这些理由。
- 解释属性随 task 和 prompt 显著变化，因此不存在一套 explanation quality 标准适用于所有任务。

## 局限

人工评价容易受语言流畅性影响；数学/事实问题中的正确性与理由忠实性仍需外部验证。未来应结合 counterfactual、attribution 和 causal intervention。

**一句话评价**：论文提供了对 LLM explanation 风格和风险的系统观察，核心警告是“解释像人”不能替代“解释忠实”。
