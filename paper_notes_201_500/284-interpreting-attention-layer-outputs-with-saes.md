# 284. Interpreting Attention Layer Outputs with Sparse Autoencoders

- **Authors:** Connor Kissane, Robert Krzyzanowski, Joseph Isaac Bloom, Arthur Conmy, Neel Nanda
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2406.17759
- **Tags:** Attention SAE, Polysemantic Heads, Induction Heads, Feature Attribution

## Introduction

SAE 多数用于 residual stream 或 MLP output，但 attention layer output 也混合了长程上下文、短程上下文和 induction 信息。论文要回答 attention outputs 能否被 SAE 稀疏、忠实、可解释地分解，并用这些 feature 理解 GPT-2 中看似冗余的 induction heads。

## Method / Framework

作者在 GPT-2 Small、Gemma-2B、GELU-2L 等多模型/多层的 attention output 上训练 SAEs，检查 L0 sparsity、cross-entropy recovery 和 live feature interpretability。进一步做 feature family analysis、head-to-feature attribution 与 recursive direct feature attribution，把任意 prompt 的 attention computation 追到 feature，再分析 long-prefix/short-prefix induction motif。

## Baselines / Comparisons

baseline 是原始 attention output、未分解 head-level analysis、SAE reconstruction 与不同模型/层的 Attention Output SAE。评价包括 average active feature 数、CE recovery、人工/自动 feature interpretability，以及对 GPT-2 Small 每个 attention head 的 feature composition，而非只比较 SAE reconstruction loss。

## Experiments / Findings

Attention Output SAEs 常能保持很稀疏且忠实：GPT-2 Small 多层 L0 约 8-21，CE recovery 约 76%-99%，抽样 live features 的 interpretability 多在 60%-97%。特征家族覆盖 long-range context、short-range context 和 induction；对 GPT-2 Small 的 qualitative study 估计至少 90% heads polysemantic，解释了为什么按 head 看会显得多个 induction heads 冗余。

## Ablation / Error Analysis

feature attribution 比只看 head 输出更细，但 SAE feature 可能 splitting、overlap 或受解释者主观判断影响。重构忠实不等于 feature causal，必须结合 recursive attribution、activation replacement 和 head ablation；不同层的 L0/CE trade-off 也不能直接横向当作解释质量。

## Limitations

主要证据是 feature qualitative inspection 和有限 causal tests，不是完整的 attention circuit 自动复原。SAE 训练成本随模型/层增长，90% polysemantic 的估计依赖 feature sample 和定义；attention output 的解释也不涵盖 MLP/residual 其余路径。

## 两句话总结

论文证明 attention output 同样可以被 SAE 分解成稀疏、可解释 feature，并用它们揭示长短上下文与 induction 的细粒度组合。它把“一个 head 一个功能”的粗粒度观念推进到 polysemantic head 内部 feature，但仍需更强的因果验证。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Attention Output SAE、Table 1、feature families、induction analysis、recursive attribution 与 limitations。

