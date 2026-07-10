# 206. Interpreting Object-level Foundation Models via Visual Precision Search

- **Authors:** Ruoyu Chen, Siyuan Liang, Jingzhi Li, Shiming Liu, Maosen Li, Zhen Huang, Hua Zhang, Xiaochun Cao
- **Venue:** CVPR 2025
- **Paper:** https://doi.org/10.1109/CVPR52734.2025.02796
- **Tags:** Visual Attribution, Object Foundation Model, Faithfulness, Region Search

## Introduction

Grounding DINO、Florence-2 等 object-level foundation models 同时融合视觉和文本，传统 gradient attribution 会因 multimodal fusion 定位不准，perturbation saliency 又常产生噪声。论文要得到更少但更精确的关键图像区域，解释 grounding/detection 的成功和失败，而不是只画一张宽泛热图。

## Method / Framework

Visual Precision Search (VPS) 绕过模型内部参数，把输入划分为稀疏 sub-regions，通过 consistency score 与 collaboration score 搜索能稳定支持目标输出的区域子集。它用视觉区域的组合/边界关系逐步收缩 attribution map，并给出边界保证和适用范围分析，因此重点从“梯度大小”转向“哪些区域共同足以维持模型决策”。

## Baselines / Comparisons

在 Grounding DINO 与 Florence-2 的 visual grounding/object detection 上，用 RefCOCO、MS COCO、LVIS 评估 VPS 与 gradient-based ODAM、perturbation-based D-RISE 等方法。指标覆盖 faithfulness、localization/attribution quality、区域数量与失败案例解释，并比较正常预测和模型错误。

## Experiments / Findings

- 对 Grounding DINO，VPS 的 faithfulness 相对提升在 MS COCO/LVIS/RefCOCO 分别为 23.7%/31.6%/20.1%；对 Florence-2，在 MS COCO/RefCOCO 分别为 50.7%/66.9%。
- 相比 gradient map，VPS 更少区域即可保留目标输出；相比 D-RISE，区域更精确、噪声更少，并能指出 object-level model 在错误 grounding/detection 时依赖了哪些误导区域。
- 结果说明 foundation model 的内部 multimodal fusion 使“直接读梯度”不可靠，输入层的精确子集搜索反而能得到更稳定的行为解释。

## Ablation / Error Analysis

消融 consistency score、collaboration score、搜索粒度和区域数量。只用单区域容易漏掉组合证据，只用协同分数会保留相关但不必要的背景；搜索过细增加推理成本，过粗又失去边界。图像中遮挡、重叠物体和模型本身的错误框会让 faithfulness 变高但 localization 语义变差。

## Limitations

VPS 需要多次 query 模型，虽然不依赖梯度但计算成本可能高于一次 forward；理论边界依赖一致性定义和搜索假设。实验集中于 grounding/detection，不能直接推广到开放式 LVLM 生成、视频或真实因果解释。

## 两句话总结

VPS 把 object foundation model 的解释变成输入区域子集搜索，用 consistency/collaboration 找到少量、精确且能维持输出的视觉证据。它显著改善多种 grounding/detection attribution 指标并能解释失败，但搜索成本与行为忠实度到内部机制的鸿沟仍然存在。
