# 117. XplainLLM: A Knowledge-Augmented Dataset for Reliable Grounded Explanations in LLMs

> 人工精读笔记：EMNLP 2024。重点是 why-choose/why-not-choice 结构化解释、知识图谱 grounding 与 debugger score。

## 0. 论文信息

- **作者**：Zichen 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.432)
- **资源**：[XplainLLM](https://lmexplainer.github.io/xplainllm)
- **主题**：grounded explanation、knowledge graph、解释可靠性

## 1. Introduction：要解决什么问题

LLM 的 CoT 或 post-hoc rationale 读起来合理，却可能没有对应模型决策；attention visualization 也难以解释 why choose 与 why not choose。需要一个把答案、支持/反对因素、外部知识和质量分数统一起来的数据与生成框架。

## 2. Method / Framework：怎么解决

XplainLLM 实例包含 question-answer、why-choose explanation、why-not-choose explanation 和 debugger score。框架从 KG 中找与问题/答案相关的 nodes/edges，经 graph attention 或 prune/rank 选择解释依据，再由 explanation generator 生成 grounded text。

why-choose 解释支持所选答案，why-not 解释说明其他候选为什么不选；debugger score 对 groundedness、coverage、quality 等维度打分，用于选择更可靠的 explanation。

## 3. Baseline / 对比基线

neuron explanation、自由文本 explanation、CoT self-explanation、已有 explanation datasets；无 KG grounding 的 LLM rationale；不同 KG pruning/embedding/retrieval 方法。

## 4. Experiments / Findings：结果如何读

数据集覆盖 structured answer 与 natural language explanation，补足仅有标签或只解释最终答案的资源。人工评价显示 explanations 的平均质量约 0.89，自动评价约 1.00（具体维度依协议而异），且 why-choose/why-not 让用户更容易检查决策边界。

KG grounding 减少纯语言模型的 hallucinated rationale，并允许把 explanation 追溯到结构化节点/边；但解释框架是受控生成，不等于对任意黑盒 LLM 内部机制完成 reverse engineering。

## 5. Ablation / 机制解释

比较有无 KG、不同 explanation retrieval、debugger score 选择以及 why-choose/why-not 的组合；结果支持外部知识和结构化解释共同提升可靠性，而单独增加文本长度未必有益。

## 6. Limitations / 局限与复现注意

KG 覆盖不全或有偏时会把错误知识变成“grounded”解释；自动/LLM evaluator 可能过度打高分；数据任务与答案格式受控，真实开放生成需要更多验证。

## 7. 两句话总结

XplainLLM 用 KG 约束 why-choose/why-not explanations，并提供 debugger score，目标是让 LLM 解释既可读又能追溯证据。实验说明结构化 grounding 比自由 rationale 更可靠，但 KG 的完整性和 evaluator 偏差仍决定最终可信度。
