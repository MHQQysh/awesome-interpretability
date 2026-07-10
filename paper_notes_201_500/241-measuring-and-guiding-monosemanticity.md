# 241. Measuring and Guiding Monosemanticity

- **Authors:** Ruben Hörle, Felix Friedrich, Manuel Brack, Stephan Wäldchen, Björn Deiseroth, Patrick Schramowski, Kristian Kersting
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2506.19382
- **Tags:** SAE, Monosemanticity, Feature Localization, Steering

## Introduction

SAE 能从 LLM activation 中提取 feature，但 feature 是否真的单义、是否稳定、能否用于控制常常只凭人工案例判断。论文先提出可量化 monosemanticity metric，再用 concept supervision 指导 SAE 学到更局部、更 disentangled 的 feature，连接“测量可解释性”和“改进可控性”。

## Method / Framework

提出 Feature Monosemanticity Score (FMS) 衡量 feature top activations 的概念一致性、稀疏性与稳定性，并设计 Guided SAE (G-SAE) conditional loss，把已知 concept signal 作为 feature localization/disentanglement 约束。实验同时评估 feature detection、steering strength、quality degradation、RIS/ROS 类稳定性与不同层/领域。

## Baselines / Comparisons

比较 vanilla SAE、不同 feature supervision、constant steering 和 G-SAE，在 toxicity、writing style、privacy 等概念上测试。指标包括 FMS、feature isolation、concept detection accuracy、activation steering 效果、输出质量与跨设置稳定性。

## Experiments / Findings

- G-SAE 在多个概念域提高 FMS，并在不明显损害 general capability 的情况下得到更可靠的 concept detection/steering。
- 概念监督比只追求 sparsity 更能定位 feature；constant steering 与 adaptive/concept-guided steering 的对照显示按输入调整强度更稳。
- 论文把 monosemanticity 视为可优化的 frontier，而非 SAE 训练后的静态属性，提供了从 feature 评估到控制的闭环。

## Ablation / Error Analysis

消融 concept loss、SAE type、层位置、sparsity 和 steering coefficient。过强监督会把相关概念硬塞进单 feature，过稀疏会损失分布式信息；FMS 高也可能来自数据模板而非模型真正语义。

## Limitations

FMS 依赖 concept annotations/labels，不能覆盖未知 feature；steering 可能改变无关风格和安全行为。实验概念和模型规模有限，跨模型、跨语言和因果 circuit 验证仍需推进。

## 两句话总结

论文提出 FMS 衡量 monosemanticity，并用 Guided SAE 的 concept-conditioned loss 同时提高 feature isolation 和 steering 可靠性。它把可解释性从“看起来单义”变成可测可导的目标，但 metric 与标签依赖仍是关键限制。
