# 213. A Closer Look at the Few-Shot Adaptation of Large Vision-Language Models

- **Authors:** Julio Silva-Rodríguez, Sina Hajimiri, Ismail Ben Ayed, Jose Dolz
- **Venue:** CVPR 2024
- **Paper:** https://doi.org/10.1109/CVPR52733.2024.02235
- **Tags:** Few-Shot Adaptation, CLIP, Domain Generalization, Model Selection

## Introduction

few-shot adapter 论文常在每个目标任务用大量 labeled validation data 和 grid search 调超参，因此报告的提升不一定能在真实低标注/域漂移环境复现。作者先指出现有方法对模型选择的依赖、甚至在 distribution shift 下不如 zero-shot，再提出一个不需要每任务调参的 class-adaptive linear probe。

## Method / Framework

CLAP（CLass-Adaptive linear Probe）从冻结 CLIP features 出发学习线性分类器，并用按类别自适应的平衡项处理 few-shot 类别不平衡和过拟合。该平衡项通过为此定制的 Augmented Lagrangian 优化，不依赖大量验证样本；ZS-LP 作为固定配置的 linear-probe baseline，强调适配效率和跨任务稳定性。

## Baselines / Comparisons

在 ImageNet、Caltech101、OxfordPets、StanfordCars、Flowers102、Food101、FGVC-Aircraft、SUN397、DTD、EuroSAT、UCF101 十一个数据集上测试 K=1/2/4/8/16。比较 zero-shot、random-init linear probing、CLIP-Adapter、TIP-Adapter、CrossModal-LP、TaskRes，以及 CoOp/ProGrad/PLOT；另以 ImageNet 适配、ImageNetV2/Sketch/A/R 测域泛化。

## Experiments / Findings

- 在固定配置、无 validation set 的公平协议下，CLAP 平均 K=1/2/4/8/16 达到 62.79/66.07/69.13/72.08/74.57，超过同协议的多种 adapter 与 ZS-LP。
- K=16 时 CLAP 仍略高于 TaskRes(e) 的 74.42；收益来自不把超参选择绑定到单一数据集，而不是额外训练大模型。
- 域漂移实验说明很多 adapter 在 source 上调得很好却在 ImageNet-A/R 等 target 上下降，CLAP/ZS-LP 的固定配置更稳；这重新定义了 few-shot adaptation 的评价重点，即 transfer robustness 而非单任务峰值。

## Ablation / Error Analysis

消融 class-adaptive balancing、Augmented Lagrangian、backbone 和 shot 数。没有自适应项时少数类容易被多数类支配，逐任务 grid search 虽提高 in-domain 分数却放大 domain drift；不同 CLIP backbone 的 feature 质量仍会成为性能上限。

## Limitations

CLAP 依赖冻结 CLIP 表示，无法解决预训练视觉概念缺失；线性 probe 对复杂任务和极少 shot 的估计仍可能不稳定。域泛化只覆盖 ImageNet 变体，且“无需验证集”协议与实际部署中可能拥有少量 validation 的场景不完全相同。

## 两句话总结

CLAP 认为 few-shot adapter 的关键问题不是缺少更复杂模块，而是现实中没有足够验证集来为每个任务调参，于是用 class-adaptive linear probe 和 Augmented Lagrangian 固定优化。它在 11 个数据集和域漂移下更稳，但能力仍受 CLIP 表示和线性模型假设限制。
