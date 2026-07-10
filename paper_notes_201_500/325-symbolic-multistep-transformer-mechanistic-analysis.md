# 325. A Mechanistic Analysis of a Transformer Trained on a Symbolic Multi-Step Reasoning Task

- **Authors:** Jannik Brinkmann, Abhay Sheshadri, Victor Levoso, Paul Swoboda, Christian Bartelt
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2402.11917
- **Tags:** Symbolic Reasoning, Deduction Heads, Backward Chaining, Circuit

## Introduction

行为 benchmark 只能说明 transformer 能答对多步推理，不能说明它是否真的建立了可复用算法。本文训练一个可控 symbolic graph reasoning transformer，反向工程 edge token、goal movement、backward chaining 和 deduction heads。

## Method / Framework

模型在序列中表示 graph edges 和 goal，第一层 attention 将 source token 信息复制到 target token，形成 edge-token concatenation；后续 deduction heads 搜索当前 target node 对应的 edge，复制 source，沿图向后推理。作者用 linear probes 预测 edge/goal、activation/path ablation 和不同路径长度分析 backward chaining、parallelization 与 leaf-node fallback。

## Baselines / Comparisons

比较 full model、不同 head/edge ablation、linear probe 对 position [A]/[B]/[P] 的预测能力，以及 backward chaining 与 heuristic leaf-node fallback。指标是 multi-step answer accuracy、probe F1、logit contribution 和 circuit component necessity。

## Experiments / Findings

edge concatenation 让 target position 含有 source 信息，deduction head 可执行 [A][B]... [B] -> [A] 的一步回溯；probe F1 显示从 [B] 位置预测 edge 可达 1.00、从 path 位置预测 goal 也达 1.00，而 [A] 位置较低。模型在目标距离较长时通过 backward chaining/parallelization 组合多步，并以 leaf-node heuristic 作为 fallback。

## Ablation / Error Analysis

第一层复制机制和后续 deduction heads 需要分别消融；只看 attention pattern 会漏掉 edge embedding 的 token composition。路径长度、目标距离、树结构和 token concatenation 是关键变量；heuristic fallback 可能在未学会完整推理时产生表面正确答案。

## Limitations

symbolic task 的图结构和 tokenization 高度可控，不能直接代表自然语言 reasoning；probe 的高 F1 也不等于每个推理路径都因果必要。更复杂的 cyclic graph、噪声事实和开放域知识需要额外验证。

## 两句话总结

论文展示了一个 transformer 如何通过 edge-token concatenation、deduction heads 和 backward chaining 执行可复用的多步符号推理。它把“多步答案”分解为可定位的 token movement 与 head circuit，但 toy task 的结构限制了自然语言外推。

## Evidence note

本笔记逐段核对 arXiv PDF 的 symbolic task、Figure 1、linear-probe Table 1、deduction-head analysis 和 conclusion。

