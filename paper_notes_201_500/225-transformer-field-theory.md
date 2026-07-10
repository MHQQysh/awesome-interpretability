# 225. Transformer Field Theory: A Response-Theoretic Approach to Mechanistic Interpretability

- **Authors:** David N. Olivieri, Antonio F. Pérez Rodríguez
- **Venue:** arXiv 2026
- **Paper:** https://arxiv.org/abs/2605.25225
- **Tags:** Mechanistic Interpretability, Activation Patching, Response Theory, Green Function

## Introduction

activation patching、causal tracing 和 steering 通常被当作彼此分离的干预技巧，难以预测一个局部改动会怎样沿层和 token 传播。论文把固定 forward pass 的 residual stream 看成随 layer depth 和 token position 变化的 Transformer field，尝试用 response theory 统一描述 patch effect。

## Method / Framework

局部 activation patch 被形式化为 Transformer field 的 source insertion；一阶 sensitivity field 预测局部干预响应，Green function 描述响应向后续层/token 的传播，patch selection 则写成 adjoint inverse problem。作者在 GPT-2-style autoregressive Transformer 上构造 forward response objects、sliced Green operators 和高敏感 site。

## Baselines / Comparisons

论文主要与 activation patching、path patching、causal tracing、steering direction 的概念框架比较，并在不同 layer-token site、source locality、响应幅度和 prompt displacement 上做理论/数值实验。指标是线性预测误差、传播结构、敏感性排序和局部干预可重复性，而非下游任务 SOTA。

## Experiments / Findings

- 局部干预存在 bounded local linear regime，一阶 sensitivity 能跨 layer-token site 预测 patch effect；这为“干预前先估计影响”提供可计算对象。
- 局部 source 会沿 Transformer field 产生结构化、各向异性的传播，高敏感 site 与 sliced Green operator 能压缩响应描述。
- prompt-induced field displacement 对 answer behavior 有部分迁移，提示 residual trajectory geometry 可能是跨 prompt 机制分析的桥梁，但并非全部行为都能线性外推。

## Ablation / Error Analysis

关键对照是 source 大小/位置、局部线性范围、层-token 切片和 Green operator 近似。干预过大时线性模型失效，远距离传播可能受非线性 attention/MLP 改写；低敏感 site 的小误差累积也会让 patch selection 不稳定。

## Limitations

当前验证集中于 GPT-2-style、固定 forward pass 和局部响应，尚未证明理论能解释大规模 instruction model 的长链行为。Green function 近似与 source 定义仍依赖模型、prompt 和坐标选择，数学响应对象也不等于人类可读概念。

## 两句话总结

Transformer Field Theory 把 activation patching 重新表述为 residual field 上的局部 source，并用 sensitivity/Green function 预测干预传播。它为 mechanistic interpretability 提供统一的 response-theoretic 语言，但现阶段仍是局部 GPT-2 风格实验，远未覆盖 frontier LLM 的全局机制。
