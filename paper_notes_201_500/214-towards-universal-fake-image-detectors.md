# 214. Towards Universal Fake Image Detectors that Generalize Across Generative Models

- **Authors:** Utkarsh Ojha, Yuheng Li, Yong Jae Lee
- **Venue:** CVPR 2023
- **Paper:** https://doi.org/10.1109/CVPR52729.2023.02345
- **Tags:** AI-Generated Image Detection, Generalization, CLIP, Nearest Neighbor

## Introduction

在 ProGAN fake 上训练 real-vs-fake classifier，换到未见过的 diffusion/autoregressive generator 时常把所有图都判为 real。论文把问题解释为 classifier 的 asymmetric sink：real 类吸收了训练时未见过的一切，而不是学到通用“伪造性”；因此需要不以 real/fake 分类目标训练的 feature space。

## Method / Framework

作者在大规模预训练 vision-language feature space 中用 nearest-neighbor classification 和 linear probing 作为简单基线，从 ProGAN real/fake 样本建立 feature bank 或训练线性头。核心思想是把检测器与 fake-specific classifier 解耦，让 CLIP 的语义/视觉表示保留跨生成器共享的异常结构。

## Baselines / Comparisons

比较 ProGAN 上训练的标准 deep classifier、已有 SOTA fake detector、CLIP/ResNet/ViT 等 feature backbone，以及 nearest-neighbor、linear probe、不同 feature-bank size。测试覆盖 GAN、diffusion 和 autoregressive generators，报告 average precision、mAP、real/fake accuracy 与平均 accuracy。

## Experiments / Findings

- 传统 classifier 在未见 diffusion 上会把 real 和 fake 几乎都判 real；例如 LDM/GUIDED/DALL-E 的平均 accuracy 约 51--52%，接近随机。
- CLIP feature 上的简单 nearest neighbor 在 unseen diffusion/autoregressive 模型上比已有 SOTA 提升约 15.07 mAP 和 25.90 accuracy，说明预训练 feature 的迁移比 fake-specific classifier 更关键。
- backbone 消融显示 CLIP:ViT-L/14 分离 real/fake 最好，ImageNet-only ResNet/ViT 较弱；论文也发现 GAN 与 diffusion 之间存在某些共享伪造模式，但尚未解释其具体来源。

## Ablation / Error Analysis

消融 backbone、预训练任务、nearest-neighbor feature bank size 和 threshold。feature bank 太小会覆盖不足，太大增加计算；图像压缩、低质量和极端生成器可能让共享 artifact 消失。好结果也可能来自 dataset/source bias，而非真正 generator-invariant 的伪造因子。

## Limitations

方法仍依赖真实/伪造样本的代表性和 CLIP 的预训练分布，未解决对抗性规避和新型生成模型持续演化。nearest neighbor 的推理成本随 bank 增长，且“共享 artifact”尚未被机制性解释；检测准确不等于能指出哪一处视觉证据是假。

## 两句话总结

论文通过分析 real sink 现象指出，fake-specific classifier 会把未见生成器错误吸收到 real 类，而 CLIP feature 上的 nearest neighbor/linear probe 更能跨生成器泛化。它提供了强而简单的 universal detector baseline，但通用伪造表征的具体语义和对抗鲁棒性仍未解决。
