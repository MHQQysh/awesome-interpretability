# 305. Grokked Transformers Are Implicit Reasoners: A Mechanistic Journey to the Edge of Generalization

- **Authors:** Boshi Wang, Xiang Yue, Yu Su, Huan Sun
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2405.15071
- **Tags:** Grokking, Implicit Reasoning, Composition, Comparison, Generalization

## Introduction

Transformer 在 parametric knowledge 上做 composition/comparison 的隐式推理可能需要长时间训练，早期过拟合不能说明模型学到了可泛化算法。论文研究 grokking 如何形成 generalizing circuit，以及不同 reasoning type 在 OOD 上为何表现不同。

## Method / Framework

作者从头训练小型 transformers 做 composition 和 comparison reasoning，记录训练 loss、ID/OOD accuracy 与内部 activation/weight/circuit 的演化。对比 memorizing circuit 与 generalizing circuit 的效率、稀疏性和表示结构，追踪模型从 overfitting 到 grokking 后如何改变功能。

## Baselines / Comparisons

比较 early memorization/overfit checkpoint、fully grokked checkpoint、composition vs comparison、ID vs OOD query/triple，并分析不同数据/训练配置。评价不仅看最终 accuracy，还看 circuit formation、component contribution 和 systematic generalization。

## Experiments / Findings

两类 reasoning 都能隐式学习，但通常要经过 extended training/grokking；composition 在 OOD examples 上不能稳定系统泛化，而 comparison 更容易泛化。机制上 generalizing circuit 逐渐形成并与 memorizing circuit 的相对效率、参数配置和数据结构相关；systematicity 取决于 circuit 是否共享可组合表示，而非仅训练 loss 下降。

## Ablation / Error Analysis

训练时间、数据覆盖、reasoning type 和 OOD 组合结构是主要消融；提前停止会把 memory shortcut 当成 reasoning，测试分布改变会暴露 composition failure。仅看 generalization accuracy 也会忽略模型切换 circuit 的阶段性，必须结合内部 component/representation analysis。

## Limitations

实验是合成 parametric knowledge 与小模型，不能直接推断自然语言 LLM 的 grokking dynamics；grokking 所需训练成本也不适合常规大模型。generalizing circuit 的完整自动定位仍有限。

## 两句话总结

论文发现隐式 composition/comparison reasoning 往往在 grokking 后才真正出现，且 comparison 比 composition 更容易跨 OOD 系统泛化。它把“答对”与“学会可泛化 circuit”分开，说明训练时长和内部电路演化是解释泛化的关键。

## Evidence note

本笔记逐段核对 arXiv PDF 的 two reasoning tasks、grokking phases、memorizing/generalizing circuits、OOD experiments 与 discussion。

