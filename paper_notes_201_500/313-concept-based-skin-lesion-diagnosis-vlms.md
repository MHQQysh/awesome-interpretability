# 313. Towards Concept-Based Interpretability of Skin Lesion Diagnosis Using Vision-Language Models

- **Authors:** Cristiano Patrício, Luís F. Teixeira, João C. Neves
- **Venue / year:** IEEE ISBI 2024
- **Paper:** https://doi.org/10.1109/isbi56570.2024.10635623
- **Tags:** Medical VLM, Concept Bottleneck, Skin Lesion, Clinical Interpretability

## Introduction

皮肤病灶分类需要医生知道模型依据了哪些可观察 clinical concepts，而不只是得到 benign/malignant label。论文研究 VLM 是否能通过颜色、纹理、边界、形状等 dermatological concepts 建立诊断与解释之间的桥梁。

## Method / Framework

框架把图像与语言/概念描述结合，先预测或对齐 lesion concepts，再用概念表示完成诊断；concept-level outputs 可由医生检查或干预。VLM 的自然语言能力用于把视觉特征和临床语义对齐，而非让模型自由生成无证据 rationale。

## Baselines / Comparisons

比较直接 image classifier/VLM diagnosis 与 concept-based diagnosis，评价分类性能、概念识别、解释可读性和 concept intervention。公开 DOI 材料缺少当前工作区可逐表核对的完整正文，因此不写未经确认的模型/数据集数值。

## Experiments / Findings

论文主张 concept bottleneck 能提供更接近临床语言的中间输出，让模型错误可按颜色/边界/纹理等概念调试；诊断性能与 interpretability 需要共同评估，不能以生成解释流畅替代 clinical concept correctness。

## Ablation / Error Analysis

concept vocabulary、concept annotation quality、是否允许 free latent、图像质量和 concept-to-diagnosis mapping 是关键消融。concept 漏标或医学概念相关性会让 bottleneck 过强，模型可能学会 shortcut；语言描述也可能把视觉不确定性变成确定性陈述。

## Limitations

医学数据、标签和群体分布有限，不能直接外推临床部署；concept supervision 成本高，模型输出还需要 dermatology expert validation。公开材料不足以确认完整的外部验证与统计结果。

## 两句话总结

该研究尝试用 VLM 将皮肤病灶诊断拆成医生可理解的视觉-临床 concepts，从而让错误定位和干预成为可能。它的价值在于把解释约束到医学概念，但 concept 标注、分布偏差和临床验证仍是决定性问题。

## Evidence note

基于 IEEE DOI/公开摘要整理；全文实验表格未获得，已明确标注未复核的具体指标。

