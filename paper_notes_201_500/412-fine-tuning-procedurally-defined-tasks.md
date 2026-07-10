# 412. Mechanistically Analyzing the Effects of Fine-tuning on Procedurally Defined Tasks

- **Authors:** Samyak Jain, Robert Kirk, Ekdeep Singh Lubana, Robert P. Dick, Hidenori Tanaka, Edward Grefenstette, Tim Rocktäschel, David Scott Krueger
- **Venue / year:** ICLR, 2024
- **Paper:** https://arxiv.org/abs/2311.12786
- **Tags:** `fine-tuning` `mechanistic-interpretability` `safety` `wrapper`

## Introduction
fine-tuning 是适配和安全对齐 LLM 的常用手段，但它究竟删除了预训练 capability，还是只改变了调用方式？如果 unsafe capability 只是被包裹/抑制，后续对看似无关任务的 fine-tuning 是否会把它重新激活？本文在可精确分析的 procedural tasks 上回答这些问题。

## Method / Framework
实验使用 Tracr compiled Transformers 和 PCFG/procedural language setups，预训练出已知 capability，再在 downstream task 上小学习率 fine-tune。作者用 network pruning、linear probing、activation analysis 和 reverse fine-tuning 检查原 capability 是否仍存在。

核心概念是 **wrapper**：fine-tuning 通常在原 capability 上学习一个很小的变换/门控，使行为看似改变，但底层能力仍可被线性 probe 读出；进一步在相关 pretraining data 上 fine-tune 可以 sample-efficiently revive 该 capability。TinyStories 提供更现实的补充验证。

## Baselines / Comparisons
比较 pretrained model、fine-tuned model、pruned/rewrapped model、reverse-fine-tuned model，以及不同 downstream/pretraining tasks。指标包括行为 accuracy、probe 可读性、删除少量 weights/neurons 后的能力变化和 revival 所需样本数。

## Experiments / Findings
三项主要结论是：fine-tuning rarely alters underlying capability；wrapper 通常很局部，可以通过剪掉少数权重/神经元恢复原行为并损害 downstream task；在相关数据上进一步 fine-tune 几步即可重新激活被包裹的 capability。TinyStories 的结果支持该现象不只存在于手工 toy circuit。

## Ablation / Error Analysis
剪枝、probe layer、learning rate、downstream task 相关性和 reverse-data 数量是主要消融。若 pretraining capability 不存在，fine-tuning 无法“复活”；若 wrapper 分布在更多权重或任务差异很大，复活成本会上升。行为上安全的模型不代表内部能力已删除。

## Limitations
程序化任务和小模型允许精确机制分析，却与真实 LLM 的知识、对齐和 RLHF 有距离。wrapper 不是所有 fine-tuning 的普遍定理，复杂任务可能真的重写表示。安全结论应理解为风险机制和警告，不是对所有 fine-tuned models 的实测保证。

## 两句话总结
本文用可控程序任务和 TinyStories 机制分析发现，fine-tuning 多数时候在已有 capability 上学习局部 wrapper，而非删除能力，少量相关数据即可将其 revive。它对安全可解释性的警告是：行为拒答或性能变化不能证明底层危险能力已经被移除。

## Evidence note
已读取 ICLR 2024/arXiv 2311.12786 v2 本地 PDF 的摘要、Tracr/PCFG 设置、wrapper、pruning、probe 和 reverse fine-tuning 结果。
