# 407. Sparse Autoencoders Find Highly Interpretable Features in Language Models

- **Authors:** Hoagy Cunningham, Aidan Ewart, Logan Riggs, Robert P. Huben, Lee Sharkey
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2309.08600
- **Tags:** `sparse-autoencoder` `polysemanticity` `features` `mechanistic-interpretability`

## Introduction
Transformer neuron 常呈现 polysemanticity：同一个神经元在多个语义无关上下文激活，导致神经元级解释碎片化。论文研究 sparse autoencoder (SAE) 能否把 residual activations 分解成稀疏、语义更纯的 feature，从而成为解释和干预语言模型的中间字典。

## Method / Framework
SAE 接收模型 activation，通过稀疏编码得到少量 feature，再用 decoder 重构原 activation。训练目标在 reconstruction fidelity 与 L1 sparsity 之间折中；每个 feature 的 top activating contexts 用语言模型/人工检查语义。

作者在 GPT-2 small 等模型的 residual stream 上训练 SAE，比较 neuron visualization、PCA/随机 dictionary 与 SAE feature，并用 feature activation ablation/steering 检查 feature 是否对输出行为有作用。

## Baselines / Comparisons
基线包括原始 neurons、稠密 autoencoder/线性字典、随机 feature 和不同 sparsity/latent expansion ratio。评价包括 reconstruction error、feature density/dead feature、解释一致性以及对模型行为的可控性。

## Experiments / Findings
SAE feature 往往比单个 neuron 更容易描述为一个语义概念、语法模式或上下文结构，支持“polysemantic neuron 可能是 superposition 下的混合坐标”的解释。增大 dictionary 宽度能找到更多细粒度 feature，但会带来 dead features 和重构/稀疏性折中。

## Ablation / Error Analysis
L1 系数、dictionary width、layer/activation 位置和训练时间是主要消融。重构好不一定解释好：SAE 可能学习到频率、tokenization 或位置等非语义 feature；过度稀疏会漏掉组合现象，过度宽 dictionary 会产生重复/碎片化 feature。

## Limitations
feature 的自然语言标签通常依赖 top examples 和人工判断，不能证明 feature 是唯一因果概念。SAE 只解释被选中的 activation slice，不自动解释跨层电路、attention routing 或输出计算。模型规模和训练数据扩大后，dictionary scaling、feature splitting 和 faithfulness 仍是开放问题。

## 两句话总结
本文用 sparse autoencoder 把语言模型的 polysemantic residual activations 分解成稀疏 feature，发现这些 feature 通常比孤立 neuron 更容易解释和干预。SAE 是机制研究的可用字典而不是最终解释，重构率、稀疏性、语义纯度和因果作用必须同时验证。

## Evidence note
已读取 arXiv 2309.08600 本地 PDF 的 SAE 目标、polysemanticity 动机、feature 分析和限制；未将 feature label 当成已经证明的概念。
