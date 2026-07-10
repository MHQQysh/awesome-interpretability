# 304. Validating Mechanistic Interpretations: An Axiomatic Approach

- **Authors:** Nils Palumbo, Ravi Mangal, Zifan Wang, Saranya Vijayakumar, Corina S. Păsăreanu, Somesh Jha
- **Venue / year:** 2024/2025 preprint
- **Paper:** https://arxiv.org/abs/2407.13594
- **Tags:** Mechanistic Interpretability, Axioms, Abstract Interpretation, Validation

## Introduction

MI 研究常用“这组组件实现了算法”描述 circuit，但不同论文对 faithful、complete、minimal、interpretable 的含义并不统一。论文的问题是能否像 program analysis 一样用 axioms 明确机械解释的正确性边界，并让不同方法的验证可比较。

## Method / Framework

作者借鉴 abstract interpretation，将模型的 concrete computation 与可解释的 abstract representation/circuit 建立映射，提出一组关于 soundness、completeness、compositionality、causal relevance 和 abstraction consistency 的公理/性质。通过这些 axioms 约束 feature/circuit interpretation 如何近似模型，并设计 test/violation criteria。

## Baselines / Comparisons

论文比较常见 activation patching、ablation、feature attribution、causal scrubbing 和小型 algorithmic transformers 上的 circuit interpretation；不是在单一 accuracy benchmark 上训练新模型，而是对不同解释 claim 检查是否满足相同公理。modular addition/小算法模型提供可验证示例。

## Experiments / Findings

公理化视角能区分“组件与输出相关”“干预组件会改变输出”“解释覆盖所有相关路径”这几个不同强度的 claim；同一个漂亮 circuit 可能满足局部 causal relevance，却不满足 completeness/minimality。该框架把 interpretation validation 从 ad-hoc plot/metric 提升为可陈述、可反驳的 specification。

## Ablation / Error Analysis

不同 corruption、baseline distribution、intervention granularity 和 output metric 会改变 soundness/causal 结论；只做正向 patch 而没有 negative control 容易违反 completeness/necessity。公理之间也可能冲突：过度追求 minimality 会丢掉冗余但真实的路径，过度完整会得到不可读的大图。

## Limitations

公理是否适合自然语言、开放域、大模型和 superposition feature 仍需研究；形式满足不自动保证人类可理解或有用。框架增加了验证成本，且许多 axioms 需要定义任务分布和可接受误差。

## 两句话总结

论文用 abstract-interpretation 思想为 mechanistic interpretation 提出 soundness、completeness、causal relevance 等验证公理，明确不同解释证据的强弱。它的价值是让 circuit claim 可反驳、可比较，但公理落到真实大模型和可读性之间仍有距离。

## Evidence note

本笔记逐段核对 arXiv PDF 的 axiomatic framework、abstract interpretation analogy、mechanistic validation properties 与 algorithmic examples。

