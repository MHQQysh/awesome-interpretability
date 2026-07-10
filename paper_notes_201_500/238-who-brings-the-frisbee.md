# 238. Who Brings the Frisbee: Probing Hidden Hallucination Factors in Large Vision-Language Model via Causality Analysis

- **Authors:** Po-Hsuan Huang, Jeng-Lin Li, Chin-Po Chen, Ming-Ching Chang, Wei-Chao Chen
- **Venue:** WACV 2025
- **Paper:** https://doi.org/10.1109/WACV61041.2025.00597
- **Tags:** LVLM Hallucination, Causal Analysis, Foreground-Background, Intervention

## Introduction

LVLM 可能在没有 frisbee 的图像中因为 sky、tree、grass 等上下文或 foreground-background 结构生成 frisbee；仅统计 hallucinated words 无法说明隐藏诱因。论文构建 hallucination probing system，研究对象、上下文、文本 prompt 与 saliency 之间的因果关系，并用干预阻断诱导路径。

## Method / Framework

作者在 AMBER 等数据上统计 single/co-occurring hallucinated words 与 non-hallucinatory inducing words，构造 semantic/non-semantic image pasting/inpainting 和 FGBG prompt intervention。框架通过 image、text、embedding 三类 intervention 比较 hallucination causal effect，再测试改变 embedding/提示是否能减少输出幻觉。

## Baselines / Comparisons

在 InstructBLIP、mPLUG-Owl2 上与现有 hallucination mitigation 方法比较，指标包括 CHAIR sentence/instance、HAL、Cog、Cover 和生成质量。还比较 semantic vs non-semantic pasting/inpainting、原始 prompt 与 FGBG prompt，并用 ground-truth region 作为 hard upper bound。

## Experiments / Findings

- FGBG prompt 在 InstructBLIP/mPLUG-Owl2 上的 CHAIR 为 5.6，HAL 为 27.8/26.5，Cog 为 2.6/2.1，同时 Cover 达 53.2/54.3，减少幻觉而未简单降低描述覆盖。
- InstructBLIP 上 FGBG 相对 baseline 将 CHs 降低 31.2%、CHi 降低 9.1%；表明 context/foreground-background 不是无关背景，而可能改变 hallucination propensity。
- semantic pasting 比 non-semantic intervention 更能暴露诱导因果；embedding intervention 显示直接编辑 representation 可能比只改输出更有潜力。

## Ablation / Error Analysis

消融 inducing object、context、semantic/non-semantic intervention、FGBG prompt 和 embedding shift。错误来自复杂多对象关系、图像粘贴不自然、模型先验共现和 intervention 破坏真实视觉证据；ground-truth region upper bound 也说明部分样本几乎无法仅靠提示修复。

## Limitations

AMBER 和 frisbee-style cases 不能覆盖所有 LVLM hallucination；prompt/image intervention 的收益可能依赖模型和数据。因果分析仍以 saliency/行为变化作中介，未完全定位训练期间形成的内部 circuit。

## 两句话总结

论文用 causal probing 证明非幻觉对象和 foreground-background context 可能诱发 LVLM 生成不存在物体，并用 FGBG/embedding intervention 降低 hallucination。它把“背景共现”变成可干预假设，但跨模型、跨任务和内部机制验证仍不足。
