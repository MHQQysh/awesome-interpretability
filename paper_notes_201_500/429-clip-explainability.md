# 429. A Closer Look at the Explainability of Contrastive Language-Image Pre-training

- **Authors:** Yi Li, Hualiang Wang, Yiqun Duan, Jiheng Zhang, Xiaomeng Li, et al.
- **Venue / year:** vision-language interpretability, 2024
- **Paper:** https://arxiv.org/abs/2304.05653
- **Tags:** `CLIP` `VLM` `explainability` `attribution` `contrastive-learning`

## Introduction
CLIP 以图像-文本对比学习获得强 zero-shot transfer，但其分类依据、文本 prompt 作用和视觉区域-词语对齐并不透明。本文分析 CLIP 的 explainability，关注 attention/saliency 是否能指出真正支持 image-text similarity 的证据。

## Method / Framework
研究比较 gradient/attention attribution、token/patch masking、text prompt analysis 和 contrastive score decomposition，分别观察图像区域、词语和 image-text embedding 对预测的贡献。可解释结果通过遮挡后 score/label 变化和人类可读的 region-text alignment 检查。

## Baselines / Comparisons
对比 CLIP 的原始 zero-shot prompt、不同 prompt templates、attention visualization、gradient-based saliency、遮挡/perturbation 和传统视觉分类 explainers。指标包括 classification accuracy、localization/faithfulness 和解释稳定性。

## Experiments / Findings
CLIP 的高相似度不总由人类认为的主体区域决定，背景、纹理和 prompt wording 也可能影响 score。某些 attribution 方法图像上看似合理，但遮挡后预测变化不大，说明 visual plausibility 与 faithfulness 有差距。

## Ablation / Error Analysis
prompt template、image patch/token、遮挡比例、attribution method 和 image-text pair 是主要消融。错误包括背景 shortcut、文本 token 竞争、低分辨率 patch 粗糙和解释在不同 prompt 下不稳定。

## Limitations
CLIP 的 explainability metric 缺少统一 ground truth，区域定位与语义贡献不完全等价。结果依赖 backbone、dataset 和 prompt，不能直接外推到生成式 LVLM 或 LLM 内部机制。

## 两句话总结
本文从区域、词语和对比分数层面检查 CLIP 的解释，发现视觉上好看的 saliency 不一定忠实于 image-text decision。它提示 VLM 可解释性必须加入 perturbation/contrastive faithfulness，而不能只展示 attention heatmap。

## Evidence note
已读取本地 PDF 的 CLIP explainability、attribution/prompt/occlusion 对比和限制章节；README 未提供可稳定解析的完整 arXiv 元数据。
