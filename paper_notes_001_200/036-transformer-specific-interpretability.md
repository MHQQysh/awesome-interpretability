# 036. Transformer-specific Interpretability

- **作者 / venue**：tutorial；EACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.eacl-tutorials.4/)
- **类型**：Transformer 可解释性方法教程

## 1. Introduction

模型无关的 gradient、occlusion 和 post-hoc explanation 可以用于很多模型，但对 Transformer 的层、head、token、residual stream 和 attention 结构利用不足。教程的核心观点是：Transformer-specific methods 不是把通用 XAI 换一个名字，而是利用架构数学结构来回答 token-to-token interaction、context mixing、layer information flow 等问题。

## 2. 方法地图

- token-to-token influence 与 context mixing。
- attention head / layer 级交互分析。
- residual stream 和 block decomposition。
- logit lens、representation projection 与 layer-wise prediction。
- activation/causal intervention，分析某层或某 head 对输出的作用。

## 3. Baseline 对照

教程将 Transformer-specific 方法与 model-agnostic gradient/occlusion 对照：后者易用但可能不忠实、不稳定，且不能自然表达层间信息流；前者解释更贴合架构，却依赖对模型结构和张量路径的理解。

## 4. Findings

- attention weight 不是充分解释；需要结合 value、residual、MLP 和输出 logit。
- 单层/单 head 可视化容易过度简化，真正的计算往往来自多层和多模块交互。
- 不同解释方法之间存在 disagreement，跨领域迁移时稳定性不足。
- Transformer-specific analysis 提供了更有机制意义的对象，但实验设计、控制组和干预验证比画图更重要。

## 5. 使用建议

读机制论文时，先明确解释目标是 information encoding、causal contribution 还是 circuit reconstruction；再选择 probing、lens、patching 或 head analysis。不要把 tutorial 中的方法清单当作每个方法都已被证明 faithful。

**一句话评价**：这篇教程建立了 Transformer 可解释性的方法坐标系，帮助区分通用解释、架构特定分析和真正的因果机制证据。
