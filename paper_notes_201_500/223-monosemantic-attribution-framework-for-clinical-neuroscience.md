# 223. A Monosemantic Attribution Framework for Stable Interpretability in Clinical Neuroscience Transformer-Based Language Models

- **Authors:** Michail Mamalakis, Tiago Azevedo
- **Venue:** arXiv 2026
- **Paper:** https://arxiv.org/abs/2601.17952
- **Tags:** Monosemanticity, Sparse Autoencoder, Clinical NLP, Attribution Stability

## Introduction

临床神经科学语言模型的 attribution 既要指出诊断相关词，又要在 IID/OOD 和不同任务中稳定；传统 gradient/attention explanation 往往稀疏性、鲁棒性和临床可读性互相冲突。论文把 sparse autoencoder (SAE) 的 monosemantic feature extraction 与 attribution optimizer 结合，研究解释是否能跨分布稳定。

## Method / Framework

框架先从 Transformer activations 提取 SAE features，再用 feature-level attribution/optimization 产生解释，并可加入 geometry-aware 约束。比较 Activation-SAE、Layer Conductance、TEO 及 TEO-SAE/TEO-UMAP 等组合，在 binary Alzheimer/control 与 three-class clinical tasks 上按 sparseness、RIS（输入随机化稳定性）和 ROS（模型随机化稳定性）共同评价。

## Baselines / Comparisons

对照是经典 attribution、无 SAE 与不同 feature-learning/geometry optimizer；设置包含 IID 和 OOD 数据集、二分类与三分类。指标不是只看稀疏 heatmap，而是同时报告 attribution sparsity、RIS/ROS stability、诊断有效性和 cohort-level marker coherence。

## Experiments / Findings

- TEO-SAE 在 binary OOD 中整体得到最低 RIS/ROS，说明 SAE 并非只让解释更稀疏，也能提高跨数据集稳定性；TEO-UMAP 可用略高稳定性代价换取更高 sparsity。
- Activation-SAE 在部分设置反而提高 RIS/ROS，说明“有 SAE”不自动等于稳定，解释优化器和几何约束决定最终质量。
- 临床案例出现连贯 diagnostic markers，并能形成低维 cohort-level explanation；作者把稳定性-稀疏性 trade-off 作为临床可审计性的核心结果。

## Ablation / Error Analysis

消融 SAE、TEO、UMAP/geometry constraint、任务类别与 IID/OOD。稀疏性单独最大化可能挑选不稳的 token，稳定性过强又会抹掉诊断细节；临床术语、数据集偏差和标签噪声会使 monosemantic feature 的解释过度泛化。

## Limitations

临床数据规模和任务范围有限，SAE feature label 仍依赖人工/自动解释，不能证明 feature 是病理机制。RIS/ROS 是 attribution 稳定性 proxy，不等于医生认同或 causal biomarker；跨医院、语言和模型架构的泛化尚待验证。

## 两句话总结

论文用 SAE monosemantic features 和解释优化器在临床 Transformer 上建立“稀疏且稳定”的 attribution 框架，并显式测试 IID/OOD 稳定性。TEO-SAE 的结果支持可控的 sparsity-stability trade-off，但临床因果性与跨医院有效性仍需验证。
