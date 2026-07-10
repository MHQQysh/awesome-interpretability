# 230. ViT-Explainer: An Interactive Walkthrough of the Vision Transformer Pipeline

- **Authors:** Juan Manuel Hernandez, Mariana Fernández-Espinosa, Denis Parra, Diego Gómez-Zar
- **Venue:** arXiv 2026
- **Paper:** https://arxiv.org/abs/2604.02182
- **Tags:** Vision Transformer, Interactive Visualization, Logit Lens, Education

## Introduction

ViT 将图像切成 patch tokens，再经过 attention、residual 和 MLP 层，初学者很难把单个组件图和最终分类连起来；已有工具常只展示 attention 或面向专家的 dashboard。ViT-Explainer 的问题是能否用 guided walkthrough 让用户沿完整 inference pipeline 追踪中间表示如何演化。

## Method / Framework

这是 web-based interactive system：先动画展示 patch tokenization，再展示 multi-head self-attention、residual connections 和 MLP transformations，最后用 vision-adapted Logit Lens 逐层显示 class prediction。系统同时提供 guided mode 与 free exploration，可将 patch-level attention 投影回 2D 图像区域并对比层间变化。

## Baselines / Comparisons

论文与孤立 attention viewer、NLP Transformer visualization 和专家型解释工具在功能覆盖上比较，并以 ViT classification pipeline 做 walkthrough。用户研究评估学习易用性、理解完整流程和探索中间表示的体验，而不是把工具当作新的分类模型 benchmark。

## Experiments / Findings

- 六位参与者的 user study 表明系统容易学习和使用，guided walkthrough 能帮助用户把 patch、attention、residual/MLP 与最终 logit 联系起来。
- vision-adapted Logit Lens 让用户看到分类信号何时在层间形成，attention overlay 则把 abstract token interaction 映射回图像位置，适合作为教学和初步假设生成工具。
- 端到端流程比只看某一层 heatmap 更能揭示表示演化，但可视化的“重要区域”仍需通过遮挡/干预验证。

## Ablation / Error Analysis

功能级比较围绕有无 guided mode、patch overlay、layer-wise logit tracing 和 free exploration。去掉任一环节会使用户只能看到局部现象；注意力聚合、降采样和 Logit Lens 的线性读出会产生解释偏差，尤其在多目标/背景相关图像中。

## Limitations

用户研究样本很小，易用性结果不能代表专家或大规模课堂；系统主要服务 ViT 分类，未覆盖生成式视觉语言模型。interactive visualization 是解释入口而非因果机制，用户仍需设计 intervention 来检验它展示的故事。

## 两句话总结

ViT-Explainer 把 patch token、attention、residual、MLP 和逐层 Logit Lens 串成可引导的 web walkthrough，降低理解 ViT 推理流水线的门槛。它适合教学和假设生成，但用户研究规模小，热力图与层级预测仍需因果验证。
