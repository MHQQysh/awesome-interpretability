# 026. IvRA: A Framework to Enhance Attention-Based Explanations for Language Models with Interpretability-Driven Training

- **作者 / venue**：Sean Xie, Soroush Vosoughi, Saeed Hassanpour；BlackboxNLP 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.blackboxnlp-1.27/)
- **任务**：训练 attention，使其同时满足 simulatability、faithfulness、consistency 等解释标准

## 1. Introduction

Attention heatmap 很容易被当作解释，但 attention weight 是否真的反映模型决策依据一直有争议。作者不满足于 post-hoc 地把已有 attention 画出来，而是提出直接在训练时把 interpretability criteria 加入目标函数，让 attention 分布本身更符合可解释要求。

## 2. Method

IvRA 对 multi-head attention 做参数化，并为 query/key 投影生成 feature-wise attention distributions。训练时保留 cross-entropy loss，再加入不同解释损失：

- **Simulatability**：人或另一个模型能否依据解释预测模型输出。
- **Comprehensibility/consistency**：解释是否稳定、是否对输入扰动保持合理一致。
- **Sufficiency**：只保留解释关注的内容时，模型是否仍能完成预测。
- **Faithfulness 目标**：attention 关注的内容应与模型决策相关，而不是仅仅是视觉上集中的 token。

## 3. Baseline 与对比

- 原始 attention-based explanation。
- 只加入单一解释损失的 IvRA 变体。
- 不同 loss combination：Sim、Comp、Suff、Cons 与 cross-entropy 的组合。
- 三类 NLP tasks 上的 downstream accuracy 与 explanation criteria 对比。

## 4. Findings

- 解释驱动训练可以让 attention explanation 更 simulatable、faithful、consistent，但通常会付出一定 downstream accuracy 代价。
- 不同 criteria 的组合并不等价：加入更多损失不保证所有维度同时提升，必须观察 accuracy-interpretability trade-off。
- 表格中某些组合仍保持约 **95.4 / 91.2 / 89.9** 的任务准确率，说明解释正则化不是必然破坏性能。
- 作者将改进归因于训练期间 attention 与决策目标共同优化，而不是事后解释器对模型输出做近似。

## 5. Ablation 与局限

最重要的 ablation 是逐个移除 Sim、Comp、Suff、Cons，观察解释分数和 accuracy 的变化。结果说明每个 criteria 约束不同：sufficiency 偏向保留足够证据，consistency 偏向稳定性，simulatability 更关注用户能否复现决策。

attention 仍不是完整 causal circuit；IvRA 只约束 attention 分布，MLP、residual stream 和后续层仍可能使用未被 attention 显式突出的位置。解释指标本身的定义也会影响训练方向。

**一句话评价**：IvRA 的关键贡献是把“attention 是否是解释”从事后争论变成训练目标与多指标实验，但其解释仍需和真正的 activation intervention 对照。
