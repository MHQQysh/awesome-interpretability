# 317. BatchTopK Sparse Autoencoders

- **Authors:** Bart Bussmann, Patrick Leask, Neel Nanda
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2412.06410
- **Tags:** BatchTopK SAE, Adaptive Sparsity, Reconstruction, Feature Learning

## Introduction

TopK SAE 每个 sample 固定激活 k 个 latent，简单但不同输入的 feature complexity 不同：简单样本可能浪费 quota，复杂样本又不够。BatchTopK 的问题是能否只固定 batch-level average sparsity，让每个 sample 自适应分配 active features。

## Method / Framework

BatchTopK 将一个 batch 中所有 latent activation 排序，只保留全 batch 预算内的 top activations，因此单个样本可激活多或少 feature，平均 L0 仍受控。训练在 GPT-2 Small、Gemma 2 2B 等模型 activation 上进行，并与 per-sample TopK、JumpReLU/其他 SAE 比较 reconstruction、sparsity、dead features 和 interpretability。

## Baselines / Comparisons

baseline 是固定 k 的 TopK SAE 与 state-of-the-art JumpReLU SAE；评价包含 reconstruction loss/fidelity、平均 L0、per-sample L0 variance、feature interpretability 和 dead latent。BatchTopK 的重点是相同平均稀疏度下的性能，不是单纯让激活更多 feature。

## Experiments / Findings

BatchTopK 在 GPT-2 Small 与 Gemma 2 2B 上稳定优于 TopK reconstruction，并在平均 sparsity 不变的情况下达到接近 JumpReLU 的效果。复杂样本获得更多 feature，简单样本更稀疏，说明自适应 quota 更符合 activation complexity；方法还避免了 per-sample fixed-k 对 feature allocation 的硬约束。

## Ablation / Error Analysis

batch size、activation budget、top-k selection、平均 L0 与 feature width 是主要消融；batch composition 会影响某个 sample 得到的 quota，导致训练/推理分布变化。若 batch 太小，估计不稳定；若预算过高，稀疏性和可读性会下降。reconstruction 提升仍需 feature-level causal tests。

## Limitations

Batch-level coupling 使不同 batch 的 feature allocation 不完全可比，在线/流式 inference 需要设计 budget；大模型训练成本和自动 feature annotation 仍高。更低 reconstruction 不等于更好的 circuit explanation。

## 两句话总结

BatchTopK 把 TopK 的固定 per-sample quota 放宽为 batch-level budget，让复杂输入获得更多、简单输入获得更少 SAE features。它在 GPT-2/Gemma 上提高 reconstruction 且保持平均稀疏，但 batch dependence 和 causal faithfulness 仍需处理。

## Evidence note

本笔记逐段核对 arXiv PDF 的 BatchTopK rule、GPT-2/Gemma experiments、TopK/JumpReLU comparisons 与 limitations。

