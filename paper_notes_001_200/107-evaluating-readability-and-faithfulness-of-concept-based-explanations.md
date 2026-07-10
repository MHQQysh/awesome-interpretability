# 107. Evaluating Readability and Faithfulness of Concept-based Explanations

> 人工精读笔记：EMNLP 2024。重点是把 concept-based explanation 的“人能读懂”和“真的反映模型决策”分开测。

## 0. 论文信息

- **作者**：Meng Li, Haoran Jin, Ruixuan Huang, Zhihao Xu 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.36)
- **主题**：concept-based explanations、readability、faithfulness、解释评估

## 1. Introduction：要解决什么问题

Concept-based explanation 先把输入映射为人类可理解的概念，再用概念解释模型预测。很多工作只展示概念名称或让人类判断“是否有帮助”，但概念可能容易读懂却没有真正影响模型，或者忠实但过于技术化，难以服务用户。

论文的问题是如何同时评估 explanation readability 和 faithfulness，以及自动 concept explanation 在不同任务、模型和概念粒度下是否稳定。核心提醒是解释质量是多目标，不应把一个 scalar score 当成全部。

## 2. Method / Framework：怎么解决

作者构建 concept-based explanation 评价协议，分别收集人类对概念可读性/有用性的判断，并通过输入扰动、概念移除或模型行为变化评估 faithfulness。解释被视为从输入 feature 到 concept，再到输出 prediction 的中间结构，需要同时回答“用户能理解吗”和“模型是否真的使用它”。

论文比较不同概念生成/选择方法和不同任务的 concept set，分析概念粒度、冗余和标签质量对两项指标的影响。

## 3. Baseline / 对比基线

- **Feature attribution/rationale**：token-level explanation，作为非概念解释 baseline。
- **自动生成 concept labels**：用 LLM 或词表生成概念，比较可读性与忠实性。
- **Human concept explanations**：作为更可读但更昂贵的参照。
- **Ablation/perturbation faithfulness**：移除概念后的预测变化。
- **Random/unrelated concepts**：控制用户喜欢程度是否只是因为概念好读。

## 4. Experiments / Findings：结果如何读

实验显示 readability 与 faithfulness 不是高度等价：自然、简洁的 concept description 可能只是对 prediction 的合理化，而一个真正有 causal influence 的 concept 可能需要更具体的上下文。概念冗余和过粗粒度会让解释看起来覆盖任务，却无法预测移除后的模型变化。

人类评价更容易受到语言流畅、熟悉度和概念数量影响；因此用户报告“解释有帮助”不能替代 perturbation/causal faithfulness。自动指标在 concept set 改变、模型换 backbone 或任务换 domain 后也会改变排序。

## 5. Ablation / 机制解释

- 概念粒度、数量和冗余度 sweep，观察 readability/faithfulness trade-off。
- 人工 concept vs 自动 concept，分离语言可读性与生成成本。
- 删除 concept、替换 concept、随机 concept，检验模型预测是否真正依赖该概念。
- 用户判断与模型行为指标对照，说明二者不能互相替代。

## 6. Limitations / 局限与复现注意

- 人类 readability 判断受背景知识、界面和用户群影响。
- perturbation 可能产生 OOD input，改变模型行为却不一定证明 concept 是原始原因。
- concept vocabulary 和生成模型选择会影响解释内容。
- 评估协议需要同时报告人类和模型指标，单一结果不宜用于宣称 faithfulness。

## 7. 两句话总结

论文指出 concept explanation 的可读性和忠实性是两个不同目标，流畅的概念描述不一定来自模型真实决策。它用人类评价结合概念干预/扰动来分别测 readability 与 faithfulness，发现概念粒度、冗余和模型选择会显著改变二者的平衡。
