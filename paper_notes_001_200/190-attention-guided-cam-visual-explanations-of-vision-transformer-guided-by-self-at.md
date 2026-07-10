# 190. Attention Guided CAM: Visual Explanations of Vision Transformer Guided by Self-Attention

- **Authors:** Saebom Leem, Hyunseok Seo
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/28077
- **Tags:** Vision Transformer, CAM, Attention, Localization

## Introduction

ViT 的 patch self-attention 让 CNN 时代的 CAM 不能直接照搬：只看 attention 往往缺少类别梯度，只看 gradient 又容易得到碎片化或局部峰值。论文的问题不是“能不能画热力图”，而是弱监督条件下，解释是否真正覆盖目标物体、是否能在遮挡/扰动时反映类别决策的重要区域。

## Method / Framework

方法从目标类别的 classification logit 反向传播到各层 self-attention，收集每个 patch 的梯度贡献；同时把 self-attention 矩阵归一化，保留 patch 之间的相关性和连续结构。两类信号先组合成 feature map，再聚合为 attention-guided CAM；归一化还用于抑制少数 attention 峰值对热力图的支配，从而在多个实例或细长目标上尽量覆盖完整前景。

## Baselines / Comparisons

作者在 ViT 的弱监督目标定位上对比 Attention Rollout 与 LRP-based method，并用 ImageNet ILSVRC 2012、PASCAL VOC 2012、CUB-200 做跨数据集比较。指标包括 pixel accuracy、IoU、Dice/F1、precision、recall；另外在 ImageNet 上做 LeRF/MoRF 像素扰动，ABPC 衡量解释排序对模型输出的忠实度。

## Experiments / Findings

- ImageNet：pixel accuracy / IoU / Dice 分别为 0.7341 / 0.5212 / 0.6515，高于 Attention Rollout 的 0.6209 / 0.3597 / 0.4893 与 LRP 的 0.5863 / 0.2029 / 0.3055；recall 0.6276 也明显高于 0.4657 和 0.2176，但 precision 0.8299 低于 LRP 的 0.9110。
- PASCAL VOC：方法达到 0.7521 pixel accuracy、0.5335 IoU、0.6646 Dice、0.6647 recall；对应 Attention Rollout 的 IoU / Dice 为 0.1645 / 0.2431，LRP 为 0.1574 / 0.2348，说明完整前景覆盖是主要收益。
- CUB-200：方法达到 0.8351 pixel accuracy、0.5836 IoU、0.7220 Dice、0.6438 recall，明显超过两种对照；作者特别指出 self-attention 的 patch correlation 有助于鸟体与背景区分以及多实例覆盖。
- 像素扰动：LeRF / MoRF / ABPC 为 0.5298 / 0.1607 / 0.3691，ABPC 高于 Attention Rollout 的 0.2685 与 LRP 的 0.3404；这使“定位更准”不仅是视觉观感，也有扰动测试支持。

## Ablation / Error Analysis

核心消融应区分 gradient、attention guidance、attention normalization 和层/头聚合。只使用 gradient 容易受峰值影响而碎片化，只使用 attention 则可能把与类别无关的上下文一并高亮；归一化和 patch correlation 共同解释了多实例定位的提升。主要错误来自目标与背景高度相关、多个目标重叠、patch 分辨率不足，以及 attention 将全局背景关系传播到目标外区域；precision 低于 LRP 也体现了 recall-precision trade-off。

## Limitations

热力图仍是 post-hoc attribution，定位指标提升不能单独证明 ViT 使用了该区域，也不等于因果解释。实验主要覆盖分类 ViT 与弱监督定位，不能直接外推到 VQA 或生成式多模态模型；更换 backbone、层、head 或 patch size 时还需要重新验证归一化和聚合超参数。

## 两句话总结

Attention Guided CAM 把类别梯度与 ViT self-attention 的 patch 相关性结合，得到比单独 Rollout 或 LRP 更完整、更忠实的弱监督视觉解释。它在 ImageNet、PASCAL VOC 和 CUB-200 上同时改善定位和扰动指标，但仍需要因果干预与更广泛任务上的验证。
