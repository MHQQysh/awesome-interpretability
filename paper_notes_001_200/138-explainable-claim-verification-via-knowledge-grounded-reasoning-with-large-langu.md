# 138. Explainable Claim Verification via Knowledge-Grounded Reasoning with Large Language Models

- **Authors:** Haoran Wang, Kai Shu
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.416
- **Tags:** Claim Verification, First-Order Logic, Grounded Reasoning, Explanation

## Introduction

传统 claim verification pipeline 通常依赖大规模人工证据标注，包含 evidence retrieval、sentence selection、verdict prediction 和 justification generation。它们在复杂 multi-hop、数值推理、表格推理和开放域科学 claim 上成本高，而且 attention 或纯生成 rationale 并不一定是人类可读、可核验的理由。

论文提出的问题是：不依赖 annotated evidence，能否让 LLM 将复杂 claim 拆成可核验子命题，借助外部知识得到 grounded answers，再输出一个能辅助 fact-checker 的自然语言解释？

## Method / Framework

FOLK（First-Order-Logic-Guided Knowledge-Grounded Reasoning）包括三步：

1. **FOL decomposition。** LLM 将 claim 翻译成一阶逻辑合取式 C = p1 ∧ p2 ... ∧ pn，每个 predicate 对应一个子 claim。
2. **Knowledge grounding。** 对每个 predicate 生成 follow-up question，使用 Google Search / SerpAPI 取 top-1 结果和回答，避免直接相信 LLM 的参数知识。
3. **FOL-guided verdict。** 若所有 predicate 为 True，则 claim SUPPORT；任一 predicate 为 False，则 REFUTE。LLM 根据每个子命题的 grounded answer 做布尔推理，并生成逐项判定和最终 explanation。

这种设计把“结论为什么成立”显式拆成 predicate truth values，而不是只给一个黑盒 label。论文示例中，claim 的第一个子事实为真、第二个事件日期为假，因此合取 claim 被判 NOT_SUPPORTED，并指出具体矛盾。

## Baselines / Comparisons

作者在 HoVER（multi-hop）、FEVEROUS（文本与表格）、SciFact（开放域科学 claim）上比较 FOLK 与 GPT-3 direct prompting、CoT、Self-Ask、retrieval / evidence-based baselines，以及已有 claim verification 系统。解释质量还通过人工比较 groundedness、完整性和能否帮助 fact-checker 来评估。

## Experiments / Findings

- FOLK 在三个数据集上整体超过强 baseline，尤其在需要拆解多跳事实和数值关系时，FOL predicate 能减少 LLM 把多个事实混成一句的错误。
- 对复杂 claim，显式子命题比直接问“claim true or false”更容易定位冲突；grounded answer 也比 LLM 自身生成的 plausible answer 更可靠。
- 生成的解释包含每个 predicate 的 True / False、对应 grounded answer 和最终逻辑合取，人工评估认为它比 attention 权重或只给最终 verdict 更清楚。
- 该方法同时覆盖 multi-hop、numerical、text-table 和 scientific claim verification，说明结构化中间表示不仅是展示形式，也改变了推理路径。

## Ablation / Error Analysis

核心消融是去掉 FOL 分解、去掉 knowledge grounding、直接使用 LLM answer，以及用自然语言 CoT 替代逻辑合取。没有 FOL 时，LLM 更容易漏掉子 claim 或错误合并；没有外部检索时，解释会继承 hallucination；没有逐 predicate verdict 时，最终答案难以审计。错误仍集中在 Google top-1 结果不可靠、predicate 翻译错误、需要常识或跨文档连接的样本。

## Limitations

SerpAPI 的 top-1 搜索结果不总是正确，且系统对 Google 可用性、排名和页面内容敏感。FOL 形式需要 LLM 正确完成 claim decomposition，复杂自然语言、模糊量词和非二值真值并不适合简单合取。论文还没有证明自然语言 explanation 与真实模型内部计算完全一致，它更准确地说是 knowledge-grounded decision trace。

## 两句话总结

FOLK 用一阶逻辑把复杂 claim 拆成可核验 predicate，再用搜索得到的 grounded answer 执行显式布尔推理和解释生成。它提升了多跳、数值和科学事实核验的可审计性，但仍受 predicate 翻译、搜索 top-1 质量和外部知识覆盖限制。
