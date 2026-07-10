# 289. What Needs to Go Right for an Induction Head? A Mechanistic Study of In-Context Learning Circuits and Their Formation

- **Authors:** Aaditya K. Singh, Ted Moskovitz, Felix Hill, Stephanie C. Y. Chan, Andrew M. Saxe
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2404.07129
- **Tags:** Induction Heads, In-Context Learning, Circuit Formation, Two-Layer Transformer

## Introduction

Induction heads 被认为是 in-context learning 的核心：previous-token head 把前一 token 复制到 residual，下一层 induction head 做 match-and-copy。它们常与 loss phase change 同时出现，但为什么需要多个 induction heads、哪些子电路先形成、不同 head 如何连接仍不清楚。

## Method / Framework

作者在小型 two-layer transformer/controlled sequence tasks 中追踪 previous-token、match 和 copy subcircuits 的训练动力学，用 activation-level ablation、pattern-preserving/value-preserving intervention 和 head connectivity analysis 区分 QK match 与 OV copy。通过只保留一条 layer-1→layer-2 path，构造 many-to-many wiring diagram，再观察 heads 的 redundancy 与 phase formation。

## Baselines / Comparisons

比较 full model、单独/组合 ablated heads、pattern-preserving vs value-preserving ablation，以及不同初始化/训练阶段和 sequence distribution。指标是 accuracy、loss/phase change、head ablation effect 和 ICL induction behavior，而不是只看 attention pattern 相似度。

## Experiments / Findings

Layer-1 heads 1,2,5（previous-token heads）在 pattern-preserving ablation 后 accuracy 99.85%，value-preserving 后降至 69.87%；其余 layer-1 heads 在 pattern-preserving 时仅 34.27%，value-preserving 时 99.32%，说明前者主要参与 match subcircuit、后者主要参与 value composition。layer-2 head connectivity 是 many-to-many：例如 L1H1→L2H3、L1H2→L2H1/2/3、L1H5→L2H1/3，而非单一 head pair。

## Ablation / Error Analysis

单独 ablation 会被 head redundancy 掩盖，必须“只保留一个 layer-1 head + 一个 layer-2 head”或区分 pattern/value intervention。induction head 的出现还依赖训练数据中的 repeated token structure，不能仅凭 loss phase change 证明完整 circuit 已形成；组件可以以冗余方式承担相同功能。

## Limitations

实验模型和数据受控，未覆盖大规模自然语言训练的丰富 induction motifs；head 编号依赖 checkpoint。结果解释的是 induction circuit formation，不等同于所有 ICL 能力都由 induction heads 负责。

## 两句话总结

该研究把 induction circuit 拆成 previous-token、match 和 copy 子电路，显示 layer-1 heads 与 layer-2 heads 通过 many-to-many 路径协作而非一一对应。pattern/value 对照还说明仅看 attention pattern 会误判 head 功能，冗余和训练数据结构是形成机制的关键。

## Evidence note

本笔记逐段核对 arXiv PDF 的 induction setup、head ablation Table 1、connectivity analysis、training dynamics 与 conclusion。

