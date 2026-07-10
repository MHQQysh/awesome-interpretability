# 203. Vision Transformers Are Robust Learners

- **Authors:** Sayak Paul, Pin-Yu Chen
- **Venue:** AAAI 2022
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/20103
- **Tags:** Vision Transformer, Robustness, Attention, Distribution Shift

## Introduction

ViT 在 ImageNet 标准准确率上已很强，但其对 corruption、distribution shift 和 natural adversarial examples 的鲁棒性及原因并不清楚。论文不只比较谁的 accuracy 高，而是把 ViT 与 Big Transfer/CNN 放在六类 ImageNet 鲁棒性数据上，再通过可控实验解释 self-attention、masking、频域敏感性和 loss landscape 与鲁棒性的关系。

## Method / Framework

作者比较不同规模和预训练组合的 ViT、BiT 与 ResNet50V2，并设计六组诊断：遮挡/随机 masking、预训练规模、对高频 Fourier 扰动的敏感性、离散余弦能量分布、对抗扰动下的 loss landscape，以及 attention 相关的表示行为。这样把“ViT 更鲁棒”拆成可测的输入、频域和优化几何因素，而不是把 attention 直接当作解释。

## Baselines / Comparisons

六个数据集覆盖 ImageNet-C common corruptions、ImageNet-R semantic shift、ImageNet-Sketch、ImageNet-A natural adversarial、ImageNet-9 background dependence 等。主对照是参数量和 ImageNet-21k 预训练尽量可比的 BiT，以及 ResNet50V2；指标是 top-1 accuracy、corruption/shift 表现和诊断实验曲线。

## Experiments / Findings

- ViT 在六类鲁棒性评测上整体显著优于可比 BiT/CNN；例如较少参数、相似预训练设置下，ImageNet-A top-1 达到 28.10%，约为可比 BiT 的 4.3 倍。
- ViT 对随机 masking 和高频 Fourier 扰动更不敏感，离散余弦能量更分散；这说明 patch token 的全局交互和较弱的卷积先验可能减少对局部纹理的依赖。
- 对抗输入下更宽的能量分布和更平滑的 loss landscape 与鲁棒性一致，但这些是归因证据而非严格因果证明；预训练规模和强正则也会显著影响结论。

## Ablation / Error Analysis

消融改变模型大小、预训练数据、mask 比例、频率分量和 attention/优化设置。若只比较标准 ImageNet accuracy 会低估 robustness；若预训练数据或正则不匹配，ViT 的优势可能缩小。频域分析还不能区分模型真正使用语义结构还是仅仅对某类纹理更不敏感。

## Limitations

实验集中在图像分类和 2022 年的 ViT/BiT 规模，不能直接外推到现代 multimodal/LLM 或不同训练配方。大部分机制解释来自相关性、可视化和输入扰动；更直接的 attention/feature intervention 仍需验证，而且算力和数据规模本身是重要混杂因素。

## 两句话总结

论文用六类 ImageNet 鲁棒性数据和六组诊断实验说明 ViT 比可比 CNN/BiT 更能抵抗 corruption、语义迁移和自然对抗样本。它把优势联系到全局 attention、较低纹理依赖、频域能量分布和更平滑的损失几何，但这些解释仍不是完整因果机制。
