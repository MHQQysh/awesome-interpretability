# 307. Hallucination Is Inevitable: An Innate Limitation of Large Language Models

- **Authors:** Ziwei Xu, Sanjay Jain, Mohan Kankanhalli
- **Venue / year:** 2024/2025 preprint
- **Paper:** https://arxiv.org/abs/2401.11817
- **Tags:** Hallucination Theory, Computability, Generalization Limits

## Introduction

大量 hallucination mitigation 工作是经验性的，却没有回答 hallucination 能否被完全消除。本文提出更基础的问题：如果 LLM 是可计算模型，而真实世界 ground-truth function 也可计算，是否存在一个通用 LLM 对所有任务都不 hallucinate。

## Method / Framework

作者在 formal world 中把 hallucination 定义为 computable LLM 与 computable ground-truth function 的不一致，用 learning theory/计算理论证明可计算模型不能学习所有 computable functions。进一步对受 provable time complexity 约束的 real-world LLM，刻画某些必然 hallucination-prone tasks，并做实验验证模型在这些任务上出错。

## Baselines / Comparisons

这是理论论文，不提出 detector 或 generator baseline；比较的是 arbitrary computable solver 的能力边界、有限资源/时间约束和经验模型的表现。实验部分以不同任务/模型作为 illustrative validation，而非宣称某个 mitigation 优于 SOTA。

## Experiments / Findings

核心结论是：只要 LLM 被要求作为通用问题求解器，就不可能学习所有 computable functions，因此总会存在 ground truth 与模型输出不一致的任务。现实世界比 formal world 更复杂，所以 mitigation 可以降低风险、提高校准和 grounding，却不能把 hallucination 从通用生成系统中逻辑上消灭。

## Ablation / Error Analysis

理论依赖任务空间、模型可计算性、资源/时间约束和 ground-truth 定义；改变这些前提会改变“不可能”结论的具体形式。不能把某一 benchmark 上的零 hallucination 当作普遍证明，也不能把 formal impossibility 当作所有具体错误概率都相同。

## Limitations

理论结论说明存在性/不可避免性，不给出每个实际模型的 hallucination rate 或最佳 mitigation；形式 ground truth 与开放域知识、交互式检索仍有差距。实验验证是有限任务，不替代应用级安全评估。

## 两句话总结

论文从学习理论证明通用可计算 LLM 不可能对所有 computable tasks 都正确，因此 hallucination 是能力边界而非单纯工程 bug。实际工作仍应做 retrieval、verification、uncertainty 和领域限制，只是不能声称把开放域 hallucination 完全消除。

## Evidence note

本笔记逐段核对 arXiv PDF 的 formal definition、learning-theoretic argument、time-complexity discussion 和 empirical validation。

