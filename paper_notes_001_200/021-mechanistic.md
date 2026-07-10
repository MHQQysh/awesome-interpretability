# 021. Mechanistic?

- **作者 / venue**：Naomi Saphra, Sarah Wiegreffe；BlackboxNLP 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.blackboxnlp-1.30/)
- **类型**：概念与社群分析；讨论 mechanistic interpretability 术语在 NLP 中的含义漂移

## 1. Introduction：为什么“mechanistic”需要被澄清

Mechanistic interpretability 在语言模型研究中快速流行，但不同论文用它表示的东西并不一致：有的指通过激活和权重解释模型内部，有的特指 circuit-level 因果机制，有的只要研究来自 MI 社群就称为 mechanistic。术语不清会让读者误以为不同论文提供了同等强度的机制证据。

## 2. 论文的分析框架

作者区分四种用法：

1. **最窄技术定义**：从模型内部的模块交互和因果电路出发，解释输入到输出的具体计算。
2. **较宽技术定义**：研究 activation、weight、neuron、layer 等内部对象，但不一定还原完整 circuit。
3. **文化/社群定义**：由 MI 社群、论坛、workshop 和相关研究传统产生的工作。
4. **广义 interpretability 标签**：任何研究模型内部或表征的工作都可能被称为 mechanistic。

## 3. Baseline 与比较方式

这不是实验性能论文，因此 baseline 不是模型分数，而是不同研究传统的对照：

- NLP probing、representation analysis、causal mediation 与 MI circuit work 的研究目标不同。
- probing 能读出信息，但不自动说明信息被模型用于决策；因此不能和完整因果机制等价。
- 计算机视觉中的 circuit/feature 传统影响了 NLP MI，但语言模态不是简单的图像分类替代品。
- 论文还提醒 random embedding、结构先验和 probe capacity 都会影响“可解释信息被发现”的结论。

## 4. Findings

- “mechanistic”在 NLP 中存在语义漂移，很多工作使用了内部表征分析，却没有给出足够的因果干预来支持狭义机制结论。
- MI 社群带来了对 circuit、superposition、feature 和 activation patching 的关注，但也形成了独立术语文化，可能与 NLP 原有 interpretability 文献断裂。
- 作者主张论文应明确说明解释对象、证据强度和因果范围：是发现相关表示，还是证明某个表示对行为必要/充分。
- 术语争论本身不是重点；重点是让读者能区分“观察到一个相关 feature”和“重建模型计算过程”。

## 5. 评价

这篇文章的作用类似领域方法论基线。读任何 mechanistic interpretability 论文时，都应问：作者是否做了 intervention？是否有 controls？是否解释了 feature interaction？是否只在一个 prompt/模型上成立？

**一句话评价**：论文不是提出新解释工具，而是要求 mechanistic interpretability 研究把术语、证据和因果主张分开写清楚。
