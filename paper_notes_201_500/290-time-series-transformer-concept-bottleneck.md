# 290. Interpretability for Time Series Transformers Using a Concept Bottleneck Framework

- **Authors:** Angela van Sprang, Erman Acar, Willem Zuidema
- **Venue / year:** 2024/2025 preprint
- **Paper:** https://arxiv.org/abs/2410.06070
- **Tags:** Time Series, Concept Bottleneck, CKA, Activation Patching, Forecasting

## Introduction

时序 transformer 能预测长期序列，但其 attention/latent 很难让领域专家解释；post-hoc feature attribution 也不一定 faithful。本文不是事后寻找 concept，而是 forward-engineer 一个 concept bottleneck，让模型训练时主动形成 predefined interpretable concepts，同时保留自由 component 学习未知信息。

## Method / Framework

作者在 Vanilla Transformer、Autoformer、FEDformer 中加入 bottleneck component，并用 Centered Kernel Alignment (CKA) loss 让 bottleneck representation 对齐预先定义的概念（如 autoregressive surrogate、hour-of-day）。预测 head 只读取 bottleneck 与 free components；训练后用 activation patching 替换 concept representation，检查预测是否按 hypothesized causal role 改变。实验覆盖 synthetic data 与六个 benchmark datasets。

## Baselines / Comparisons

比较 Att bottleneck、FF bottleneck、no bottleneck、AR surrogate model 和原始 Autoformer（Wu et al.）。指标为 MSE/MAE、概念可读性、不同 transformer backbone 的 representation alignment，以及 temporal shift 下局部 intervention 的效果。

## Experiments / Findings

Autoformer 结果中 Electricity 的 Att/FF/no bottleneck MSE 为 0.231/0.207/0.280，Traffic 为 0.642/0.393/0.619，Weather 为 0.290/0.271/0.269；在 Traffic、Exchange rate、ETT 等数据上 bottleneck 优于原 Autoformer，其他数据通常在 5% 内。AR surrogate 这个简单模型在多数数据上反而很强，说明可解释 baseline 不应被忽略。activation patching 提示 bottleneck components 具有预期的独特/因果预测作用。

## Ablation / Error Analysis

Att vs FF bottleneck、是否保留 free component、concept 数量和 synthetic/real data 是主要消融。concept 预定义错误会把模型限制到错误解释；只用 domain-agnostic AR/hour-of-day 仍有效，但不能涵盖所有季节/事件因素。CKA 对齐提高可解释性却增加计算，概念 representation 与真实 causal variable 仍需 intervention 验证。

## Limitations

需要训练前决定 concepts 并提供其 representation，依赖领域知识；bottleneck 增加训练与模型复杂度。六个数据集和两个主要概念不足以证明自然语言、视觉或高维多变量场景都适用，且 activation patching 仍是局部因果证据。

## 两句话总结

该框架用 CKA loss 将时序 transformer 的部分 latent 对齐到预定义 concept，再用 activation patching 验证这些 concept 真会影响预测，从而把解释性前移到训练过程。它在保持预测性能的同时提升可解释性，但 concept 选择成本、计算开销和覆盖未知因素是主要限制。

## Evidence note

本笔记逐段核对 arXiv PDF 的 CKA bottleneck method、Table 1、synthetic/benchmark experiments、activation patching 与 limitations。

