# 370. CLIPER: A Unified Vision-Language Framework for In-the-Wild Facial Expression Recognition

- **Authors:** Hanting Li, Hongjing Niu, Zhaoqing Zhu, Feng Zhao
- **Venue / year:** IEEE ICME 2024 / arXiv 2023
- **Paper:** https://arxiv.org/abs/2303.00193
- **Tags:** `vision-language` `facial-expression` `prompt-learning` `interpretable-representation`

## Introduction
Facial expressions are compound and variable: the same class can appear as a mild smile, a laugh, or a context-dependent affect. One-hot or soft labels provide little semantic explanation and treat all examples in a class as equivalent. CLIPER asks whether language supervision can produce more fine-grained and interpretable expression representations for both static and dynamic in-the-wild FER.

## Method / Framework
CLIPER adapts CLIP to facial expression recognition and introduces Multiple Expression Text Descriptors (METD). Instead of one hand-written text prompt per class, each expression superclass has K learnable subclasses and M learnable text tokens; inference averages image-to-text cosine similarity over the subclass descriptors.

The training is two-stage: learn expression text descriptors while preserving image-text alignment, then fine-tune the image encoder so it captures facial texture details rather than CLIP's generic object semantics. Dynamic videos use straightforward temporal mean pooling, keeping the contribution focused on semantic supervision rather than a new temporal module.

## Baselines / Comparisons
Prompt-learning baselines are zero-shot CLIP, Linear Probe, Fully Fine-tuning, CoOp, and CoCoOp. FER baselines include DLP-CNN, gACNN, IPA2LT, RAN, SCN, KTN, EfficientFace, MVT, VTFF, TransFER, Face2Exp, I3D-RGB, 3D ResNet18, Former-DFER, STT, NR-DFERNet, DPCNet, and IAL.

The paper evaluates static FER on RAF-DB and AffectNet, and dynamic FER on FERV39K, DFEW, and AFEW, using WAR for static recognition and WAR/UAR for dynamic recognition. It also visualizes nearest vocabulary words and sample images associated with learned METD subclasses as an interpretability comparison.

## Experiments / Findings
On RAF-DB/AffectNet, CLIPER reaches 91.61% WAR and 66.29/61.98% on the reported AffectNet 7/8-class settings, beating TransFER's 90.91 and 66.23. On dynamic datasets it reaches FERV39K UAR/WAR 41.23/51.34, DFEW 57.56/70.84, and AFEW 52.00/56.43, exceeding the listed prior methods.

The fine-tuning ablation on FERV39K/RAF-DB reports CLIP zero-shot 21.20/16.20 and 40.16, Linear Probe 33.06/44.57 and 84.29, Fully Fine-tuning 38.68/50.13 and 90.38, and two-stage training 39.32/50.52 and 90.91. METD's nearest words include “saddest” for sad, “kindness” for happy, “roaring” for angry, and “zombies” for surprise, showing that the learned descriptors encode related semantics beyond class names.

## Ablation / Error Analysis
Varying K and M shows that multiple subclasses outperform a single descriptor; on RAF-DB, K=5/M=4 gives 91.61 WAR, while K=1/M=4 gives 90.91. Larger token length helps the larger dynamic dataset, whereas smaller length can avoid overfitting on RAF-DB. The two-stage strategy beats a direct linear probe and ordinary full fine-tuning because text guides the image encoder toward expression semantics while later tuning recovers face texture details.

The authors note that CLIP is strong for noun-like classes but weaker for abstract compound categories. METD addresses this by allowing multiple forms/intensities: happy subclasses can separate smile and laugh, and sad subclasses appear to vary with age-related semantics. Temporal aggregation is intentionally simple, so the result does not show that CLIPER solves temporal expression dynamics.

## Limitations
The framework still depends on pseudo-language descriptors learned from labels and uses mean pooling for video, leaving motion-specific reasoning underdeveloped. Some nearest words are semantically surprising, which is interpretable but not necessarily clinically valid. Dataset imbalance and class definitions, especially AffectNet's 7/8-class variants, make cross-paper comparisons sensitive to protocol.

## 两句话总结
CLIPER 用多个可学习的表达文本描述替代每类一个手工 prompt，让 CLIP 在静态和动态人脸表情识别中学习不同强度、语义和外观形式，并通过最近词和样本可视化提供解释线索。它在五个 in-the-wild FER benchmark 上超过列出的方法，但视频时序只用平均池化，语义可读性也不等于心理或临床意义。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；表 II-V 的 WAR/UAR 和 METD 可视化结论按原文记录，README 中的 ICME DOI作为会议版本入口保留。
