# 423. Uncovering Intermediate Variables in Transformers using Circuit Probing

- **Authors:** Michael A. Lepori, Thomas Serre, Ellie Pavlick
- **Venue / year:** COLM, 2024
- **Paper:** https://arxiv.org/abs/2311.04354
- **Tags:** `circuit-probing` `intermediate-variables` `mechanistic-interpretability`

## Introduction
Transformer 能在复杂任务上取得高分，但我们通常只看到输入输出，无法知道中间变量是什么。传统 probe 可以读出信息，却可能使用模型不依赖的 shortcut；本文提出 circuit probing，寻找模型内部对计算真正有用、并且位于中间机制链上的变量。

## Method / Framework
方法结合 probing、causal intervention 和 circuit localization：先在 hidden states 中训练 probe 预测候选中间变量，再干预/屏蔽这些变量，观察 downstream output 是否按预期变化。任务使用可控的 synthetic/structured reasoning，使 ground-truth intermediate variables 可定义。

## Baselines / Comparisons
对比普通 linear probing、random layer/feature、只看 final representation 的 probe、activation ablation 和完整模型。指标包括 probe accuracy、circuit faithfulness、intervention effect 与稀疏性。

## Experiments / Findings
实验表明，某些中间变量可从表示中读出并通过干预影响最终预测，支持它们是计算电路的一部分，而不是纯粹相关标签。circuit probing 将“information is present”与“information is used”区分开，提供了比单独 probe 更强的机制证据。

## Ablation / Error Analysis
层、变量、probe capacity、干预位置和 corruption 是主要消融。高容量 probe 可能从 distributed features 拼出变量却不对应模型计算；错误干预可能破坏多个路径，导致 effect 被高估。需要对随机变量、冗余路径和 out-of-distribution inputs 做控制。

## Limitations
可控任务的中间变量有明确答案，开放自然语言中的变量定义更模糊。causal intervention 成本高，且一个行为可能有多个等价电路；probe/ablation 仍不能保证找到唯一内部算法。

## 两句话总结
Circuit probing 将 probe 的可读信息与 intervention 的因果作用结合，用于识别 Transformer 中真正参与计算的 intermediate variables。它比单纯读出更接近机制解释，但仍依赖候选变量、任务设计和对冗余路径的控制。

## Evidence note
已读取 COLM 2024 本地 PDF 的 circuit probing、probe/intervention 设计、baseline 和限制章节。
