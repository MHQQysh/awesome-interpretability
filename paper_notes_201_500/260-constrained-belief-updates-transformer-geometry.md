# 260. Constrained Belief Updates Explain Geometric Structures in Transformer Representations

- **Authors:** Mateusz Piotrowski, Paul M. Riechers, Daniel Filan, Adam S. Shai
- **Venue / year:** ICML 2025
- **Paper:** https://arxiv.org/abs/2502.01954
- **Tags:** Mechanistic Interpretability, Bayesian Updating, Transformer Geometry, HMM

## Introduction

最优 next-token prediction 可以被理解为对隐藏状态/未来分布做递归 Bayesian belief update，但 attention 是并行、非负、受因果 mask 约束的加权求和。论文研究这种理论需求与架构约束如何共同塑造内部表示：为什么 transformer 会出现特定的 fractal/simplex geometry，以及 attention、OV vector、embedding 和 MLP 分别在算法上做什么。

## Method / Framework

作者使用可解析的 Mess3 hidden Markov model：三个 hidden states，参数 alpha 控制 emission 的区分度，x 控制状态转移/持久性。用这些 HMM 采样序列，训练 1-layer 和 4-layer transformer 做 next-token prediction，再对 attention 前后 residual stream 做 PCA、weight/attention pattern 分析，并把激活与理论 belief-state geometry 做 MSE 比较。理论上把最优 Bayesian update 按 transition matrix 的 eigenvalues 分解，推导受 attention 非负性限制的 constrained belief update，预测 attention decay、OV vectors、token embeddings 和多头如何配合。

## Baselines / Comparisons

这不是传统的 SOTA benchmark，而是理论预测对训练模型的 mechanistic verification。比较对象包括 unconstrained/full Bayesian belief geometry、constrained intermediate geometry、随机初始化模型和不同层数/不同 Mess3 参数的训练模型；单层与四层模型用于区分基础 attention 计算和后续 MLP/layer refinement。PCA 只是可视化/低维表示工具，核心证据是理论结构与实际 attention/权重/激活的对齐。

## Experiments / Findings

- attention 后、MLP 前的 residual 表示呈 fractal-like structure，最终表示更接近 HMM 的 probability-simplex belief geometry；训练过程中两者对理论 geometry 的 MSE 快速下降，且显著优于随机初始化。
- OV vectors 聚成对应三个 hidden-state 顶点的方向，token embedding 靠近原点；attention 权重主要由相对位置决定并按 (1-3x) 的幂衰减，token identity 主要进入 value/OV 向量。理论 Eq. 13-15 对 attention、OV magnitude 与 embedding 的关系给出可检验预测，并在训练模型中得到匹配。
- 当 transition eigenvalue ζ=1-3x 为负时，单个 attention head 不能表示交替振荡的负更新，因为 attention weights 非负；两个 head 用相反方向的 OV vectors 合成正/负分量，恢复理论的 oscillatory update。四层模型中第一层先实现 constrained belief update，后续层逐渐靠近 full Bayesian geometry。

## Ablation / Error Analysis

改变 alpha/x 会改变 emission clarity 和 transition persistence，理论预测的几何形状也随之变化；负 eigenvalue 设置下增加第二个 head 是关键结构消融。单层模型隔离了 attention，但 MLP 的非线性 warping 仍未完全解析；模型在特殊 HMM 上训练，注意力对位置的依赖与 token-specific value 的分工可能是 toy setting 的产物。representation 对理论 geometry 的接近不自动证明模型在自然语言中执行相同 Bayes circuit。

## Limitations

实验模型很小，主要分析第一 attention layer；Mess3 是有完整支持、可解析的 HMM，远离自然语言的层次性、稀疏性和长程语义。论文证明了第一层是 foundational computation，但没有解释梯度下降为何收敛到这个电路，也没有完整刻画更深层、多头和真实语料的相互作用。

## 两句话总结

论文用计算力学与 mechanistic interpretability 证明，transformer 可以把递归 Bayesian belief update 变成受 attention 约束的并行加权算法，因而产生可预测的 fractal 与 simplex 几何。它把“内部几何是什么”推进到“为什么这种几何必然出现”，但证据主要来自小型 Mess3 HMM，尚不能直接外推到自然语言 LLM。

## Evidence note

本笔记逐段核对 ICML/arXiv PDF 的 Introduction、Mess3 methodology、Sections 4.1-4.5、Figures 2-4 和 Discussion/Limitations；理论预测与机制结论按正文保留。

