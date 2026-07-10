# 280. Mechanistic Interpretability of Binary and Ternary Transformers

- **Authors:** Jason Li
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2405.17703
- **Tags:** Binary Transformer, Ternary Transformer, Modular Addition, Clock Algorithm

## Introduction

二值/三值权重和激活可以显著降低 LLM memory 与 inference cost，也有人猜测离散网络可能更容易解释。论文在 modular addition 这个可完全 reverse-engineer 的 toy task 上问：binary/ternary transformer 是否学到与 full-precision transformer 不同、或更简单的算法。

## Method / Framework

作者训练 binary、ternary 和 full-precision 1-layer transformers 做 a+b mod p，沿用 modular addition 的 Clock algorithm 分析：数字先用 Fourier features 表示成旋转，attention 搬运 a/b，MLP 组合成 a+b 的相位，unembedding 读出 logits。随后检查 Fourier representation、attention patterns、MLP weights、logit differences 和 ablation，比较离散化是否改变 circuit。

## Baselines / Comparisons

full-precision transformer 是主要 baseline，binary/ternary 是不同量化约束；任务包括不同 prime/modulus、训练精度和初始化条件。评价包含 accuracy、Fourier spectrum、component ablation、attention/MLP contribution 与已知 Clock circuit 的结构一致性，而不是只比较参数量或速度。

## Experiments / Findings

binary 和 ternary networks 学到与 full-precision 类似的 modular-addition algorithm：仍可看到 Fourier/rotation representation、attention information routing 和 MLP phase composition。由此没有证据支持“离散权重天然带来更可解释的 LLM circuit”；量化改变了参数表达和训练条件，却没有消除 algorithmic superposition。

## Ablation / Error Analysis

低精度模型的成功率对训练稳定性、modulus、初始化和网络结构更敏感；某些模型能在单个 test batch 达到 100% accuracy，但不代表跨所有输入泛化。仅比较离散权重数值无法判断机制，必须看 component ablation 与 Fourier/logit behavior。toy modular addition 也不能覆盖自然语言知识存储和长上下文电路。

## Limitations

实验规模小、任务单一，结论主要是对“binary/ternary 更容易 interpret”的反例；没有证明大型量化 LLM 与 full precision 在所有任务上共享 circuit。速度/内存收益也不是本文的系统 benchmark 主线。

## 两句话总结

论文在 modular addition 上发现 binary/ternary transformer 仍学习类似 full-precision 的 Clock algorithm，因此量化本身不会自动让 LLM 更可解释。它把低精度部署与机制透明度区分开，同时说明离散模型仍需同样的 circuit-level causal analysis。

## Evidence note

本笔记逐段核对 arXiv PDF 的 modular-addition setup、binary/ternary experiments、Fourier/attention/MLP analysis 与 conclusion。

