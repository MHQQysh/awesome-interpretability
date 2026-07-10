# 246. Interpretable Deep Learning for Polar Mechanistic Reaction Prediction

- **Authors:** Ryan J. Miller, Alexander E. Dashuta, Brayden Rudisill, David Van Vranken, Pierre Baldi
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2504.15539
- **Tags:** Chemistry, Reaction Mechanism, Interpretability, Transformer

## Introduction

大多数 reaction prediction 直接从 reactants 映射到 products，能给出结果却不能说明逐步电子转移/键变化；化学家更关心可验证的 mechanism。论文构建 polar mechanistic pathway 数据并训练能预测 elementary steps 的系统，让准确率与机理可读性同时成为目标。

## Method / Framework

提出 PMechDB 与相应的 mechanistic reaction prediction pipeline，利用反应物、产物和中间步骤预测极性反应路径；系统比较 Chemformer、two-step 架构和 hybrid ensemble，并通过中间 elementary reaction/目标原子变化展示可解释路径。

## Baselines / Comparisons

比较直接 reaction transformation 模型、Chemformer、two-step mechanism model 和 hybrid/ensemble。指标包括 PMechDB test 的 top-k accuracy、pathway target recovery，以及推理时间/计算成本，重点区分最终产物正确与 mechanism 正确。

## Experiments / Findings

- 组合生成反应数据增强训练后，机制预测表现提升；hybrid 系统在 PMechDB 达到 94.9% top-10 accuracy，在 pathway dataset 达到 84.9% target recovery。
- 逐步路径使模型预测可被化学规则与中间产物检查，比只输出最终 product 更适合发现错误和分析机理。
- ensemble/更复杂模型可能继续提升分数，但推理开销增加，解释路径的化学有效性不能仅由 top-k 命中替代。

## Ablation / Error Analysis

消融 Chemformer、two-step、hybrid、组合生成增强和 pathway supervision。步骤过多、反应物映射错误、稀有反应和机理等价路径会造成 sequence mismatch；top-k 命中也可能是产物对而路径不唯一。

## Limitations

数据依赖已知 polar mechanism 与 atom mapping，不能覆盖所有有机反应和真实实验条件；top-k/path recovery 仍是自动指标。模型解释需要化学家审阅，且 hybrid ensemble 的速度和可部署性有限。

## 两句话总结

论文把 reaction prediction 从直接产物映射推进到可检查的 polar elementary mechanism，并用 PMechDB 评估逐步路径。PMechRP hybrid 达到 94.9% top-10 和 84.9% pathway recovery，但化学有效性与真实反应条件仍需专家验证。
