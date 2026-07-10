# 278. Opening the AI Black Box: Program Synthesis via Mechanistic Interpretability

- **Authors:** Eric J. Michaud, Isaac Liao, Vedang Lad, Ziming Liu, Anish Mudide, Chloe Loughridge, Zifan Carl Guo, Tara Rezaei Kheirkhah, et al.
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2402.05110
- **Tags:** MIPS, Program Synthesis, RNN Interpretability, Symbolic Regression

## Introduction

神经网络能学会算法，但通常只给出输出，无法告诉人类实现逻辑。MIPS (Mechanistic-Interpretability-based Program Synthesis) 的问题是能否自动把一个训练好的网络内部算法蒸馏成 Python 程序，使解释从自然语言描述变成可执行、可验证的 symbolic object。

## Method / Framework

MIPS 针对学习 62 个 algorithmic tasks 的 RNN：先用 integer autoencoder 把连续 hidden dynamics 转为 finite-state representation，再用 Boolean/integer symbolic regression 拟合状态转移和输出规则，最终生成 Python code。它结合模型行为、隐藏状态和低维离散结构，不需要人工逐个写 circuit；同时与 GPT-4 code/program synthesis 结果互补。

## Baselines / Comparisons

benchmark 覆盖 base/ternary addition、string operations、counters、sorting/logic 等 62 tasks。对照是 GPT-4 直接程序合成、MIPS 自身在不同任务/不同 autoencoder-regression setting 下的成功率，另有未成功或 partial program 的人工检查。评价是生成程序能否在测试输入上复现原网络算法，而不是文字解释是否流畅。

## Experiments / Findings

MIPS 解决 32/62 个任务，其中 13 个是 GPT-4 未解决而 MIPS 解决；GPT-4 解决约 30 个，说明内部动力学证据与语言先验互补。抽出的程序可以暴露简单 RNN 的计数、有限状态、布尔逻辑和字符串操作机制，且程序在 hold-out 输入上能做行为验证。

## Ablation / Error Analysis

integer autoencoder 的离散化质量是瓶颈：hidden state 若不是近似有限状态，symbolic regression 会产生不稳定或过拟合的规则。回归搜索空间、程序长度和任务表示也决定成功率；同一个网络可能有等价程序，不能把单个 synthesized code 当作唯一真实算法。失败任务通常更复杂、状态更多或需要长程连续数值。

## Limitations

实验主要针对小型 RNN 和算法任务，不能直接处理自然语言 transformer 的高维 superposition；自动程序也可能只复现行为而未覆盖所有内部路径。MIPS 依赖离散化与符号搜索，规模、噪声和任务分布扩展仍未知。

## 两句话总结

MIPS 先把 RNN hidden dynamics 离散成有限状态，再用符号回归自动生成 Python 算法，62 个任务中解决 32 个并补足 GPT-4 的程序合成盲点。它证明机制解释可以直接产出可执行程序，但当前仍局限于小型、结构化算法和可离散的网络状态。

## Evidence note

本笔记逐段核对 arXiv PDF 的 MIPS method、62-task benchmark、Table 1、GPT-4 comparison 与 limitations。

