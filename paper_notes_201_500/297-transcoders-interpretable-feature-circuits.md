# 297. Transcoders Find Interpretable LLM Feature Circuits

- **Authors:** Jacob Dunefsky, Philippe Chlenski, Neel Nanda
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2406.11944
- **Tags:** Transcoder, Feature Circuits, SAE, MLP Interpretability

## Introduction

Transformer MLP layer是 feature circuit 分析的难点：每个 neuron 有非线性，SAE feature 又是许多 neuron 的线性组合，直接展开会得到巨大、难以解释的 circuit。Transcoder 的问题是能否用一个更宽但稀疏激活的 MLP 近似 dense MLP layer，从而把 input feature 到 output feature 的 circuit 变成可计算的 sparse graph。

## Method / Framework

Transcoder 接收一个 MLP layer input，预测该层 output，结构上是 overcomplete sparse MLP；训练后每个 transcoder feature 的 decoder/encoder 给出输入-输出 feature mapping。作者提出基于权重的 circuit analysis，把贡献因子分成 input-dependent activation 与 input-invariant weights，利用 feature graph 追踪行为的局部路径，而不必展开所有原始 neurons。

## Baselines / Comparisons

比较 direct neuron-level circuit、SAE feature decomposition、transcoder reconstruction 与原始 MLP output；在 120M、410M、1.4B 模型上测试，并在已知 task/feature circuit 上评价 faithfulness、sparsity、interpretability 和行为恢复。核心 baseline 是“直接分析 dense MLP”带来的组合爆炸，而非某个下游分类 SOTA。

## Experiments / Findings

Transcoders 能在多个模型规模上忠实近似 MLP，生成比 neuron expansion 更稀疏、可读的 feature circuits；input-dependent activation 与 input-invariant weights 的 factorization 使相同 feature path 可跨 prompt 复用。它能用于追踪 token 到 feature、feature 到 logits 的路径，并将局部 MLP behavior 与全局 circuit 分离。

## Ablation / Error Analysis

transcoder width、sparsity、reconstruction quality 和 circuit threshold 会影响图大小；过度压缩会漏掉弱但关键路径，过宽会重新引入 polysemanticity。权重-based circuit 是解释代理，必须用 activation ablation/patching 验证路径；近似 MLP 不等于原 MLP 内部唯一算法。

## Limitations

训练 transcoder 仍需大规模 activation 数据和模型访问，当前实验规模与 task 范围有限。对于跨层 residual/attention interactions，单层 transcoder graph 仍不完整；自动 feature labels 也可能存在错误。

## 两句话总结

Transcoder 用更宽、稀疏激活的 MLP 近似 dense MLP，让 feature-level circuit 可以用权重图而非巨大 neuron graph 分析。它为 SAE feature 到跨层算法提供了中间抽象，但 approximation fidelity 和真实因果路径仍需干预验证。

## Evidence note

本笔记逐段核对 arXiv PDF 的 transcoder architecture、weight-based circuit factorization、120M/410M/1.4B experiments 与 limitations。

