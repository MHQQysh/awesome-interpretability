# 216. Improving Visual Grounding by Encouraging Consistent Gradient-based Explanations

- **Authors:** Ziyan Yang, Kushal Kafle, Franck Dernoncourt, Vicente Ordonez
- **Venue:** CVPR 2023
- **Paper:** https://doi.org/10.1109/CVPR52729.2023.01837
- **Tags:** Visual Grounding, Grad-CAM, Explanation Consistency, AMC

## Introduction

预训练 vision-language model 可以通过 gradient heatmap 做 visual grounding，但如果训练目标只要求图文匹配，梯度解释可能指向无关背景。论文提出让模型的 gradient-based explanation 与人工 region annotation 一致，使解释不只是事后可视化，而成为训练视觉 grounding 能力的监督信号。

## Method / Framework

作者提出 Attention Map Consistency (AMC)，计算模型输出对图像区域的 gradient map，并用 margin-based loss 约束 heatmap 与标注 box/region mask 对齐；同时保留标准 vision-language pretraining objective。方法以 ALBEF 为基座，在 Visual Genome 的 object/region annotations 上训练，再将解释用于 Flickr30k Entities、RefCOCO+、ReferIt 等 grounding。

## Baselines / Comparisons

比较 gALBEF、Grad-CAM、MG、GbS、GAE、WWbL、InfoGround、12-in-1、VMRM，以及使用 Visual Genome detector 的方法；也比较仅 box、仅 region、attribute description 和组合监督。指标是 pointing-game/visual grounding accuracy，并报告 RefCOCO+ easy/hard 与 ReferIt。

## Experiments / Findings

- Flickr30k grounding 达 86.49（另一公平 AMC 版本 86.59），相对同等监督最佳先前方法绝对提升 5.38 个百分点；RefCOCO+ easy/hard 达 80.34/64.55。
- Table 1 中 AMC 优于 VMRM 的 81.11/58.87/50.32 以及 InfoGround 等 detector-based 方法，说明解释一致性 loss 能把语言匹配转成更精确区域关注。
- 同时使用 box、region 与 description 通常最好；attribute box 在多数数据上更准，region description 在某些 RefCOCO+ split 更有利，表明监督粒度应匹配目标任务。

## Ablation / Error Analysis

消融 AMC margin loss、cosine-distance loss、box/region supervision、描述类型和不同 image encoder。只约束某一种区域可能丢失空间/语义信息；CLIP ViT 作为视觉 backbone 优于 CLIP ResNet-101、DeiT 和 bottom-up features。背景相关、区域重叠和 box 标注噪声仍会使 heatmap 看似一致却不完全 faithful。

## Limitations

训练需要 region-level annotations，这削弱了“低成本无监督解释”的扩展性；gradient map 与 box 对齐也不保证模型因果上使用该区域。实验主要是 grounding/指代表达，不能直接覆盖开放式 MLLM generation 或多步推理。

## 两句话总结

AMC 把 gradient explanation 与人工区域标注显式对齐，使 visual grounding 模型学会在图文匹配时关注正确区域。它显著提升 Flickr30k/RefCOCO+ 定位，但需要额外框标注，且一致热力图仍不等于因果解释。
