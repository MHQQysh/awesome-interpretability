# 007. Probing Internal Representations of Multi-Word Verbs in Large Language Models

## 7. Probing Internal Representations of Multi-Word Verbs in Large Language Models

- **作者 / 发表**：Hassane Kissane, Achim Schilling, Patrick Krauss；MWE 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.mwe-1.2/)
- **方向**：语言学 probing、层级表征、非线性可分性
- **要解决的问题**：Transformer 是否在内部表征中区分不同类型的多词动词，以及这种词汇/句法信息在哪些层表现得最明显，仍缺少细粒度分析。
- **怎么解决**：论文以 BERT 为对象，研究 phrasal verbs（如 give up）和 prepositional verbs（如 look at），在词级和句子级输出上训练 probing classifier，并用 Generalized Discrimination Value 检查类别的几何可分性。
- **摘要概括**：中间层的分类准确率最高，说明相关语言结构在中间层更容易被读出。但 GDV 显示两类表示的线性可分性较弱，而分类器仍能取得高准确率，说明信息可能以非线性方式编码。
- **阅读要点**：这是“probe 能读出信息”与“信息被线性、局部、因果地编码”之间差异的典型例子。高 probe accuracy 不能单独证明模型采用了人类可解释的简单决策规则。
