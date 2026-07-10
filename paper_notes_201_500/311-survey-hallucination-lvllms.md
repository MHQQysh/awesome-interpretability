# 311. A Survey on Hallucination in Large Vision-Language Models

- **Authors:** Hanchao Liu, Wenyuan Xue, Yifei Chen, Dapeng Chen, Xiutian Zhao, Ke Wang, Liping Hou, Rongjun Li, Wei Peng
- **Venue / year:** 2024 survey
- **Paper:** https://arxiv.org/abs/2402.00253
- **Tags:** LVLM Hallucination, Visual Grounding, Evaluation, Mitigation

## Introduction

LVLM 可能把不存在的 object、错误数量、错误位置或错误属性写进回答，形成视觉事实与文本生成的不一致。本文整理 hallucination 的定义、类别、原因、benchmark、metric 和 mitigation，为后续研究提供共同语言。

## Method / Framework

survey 按 object/attribute/relation/counting/OCR 等 hallucination 类型分析视觉 encoder、projector、LLM prior、训练数据与解码的原因，再整理 detection/evaluation（如 object-level、region-level、human judgment）和 mitigation（数据、alignment、prompt、decoding、retrieval、post-hoc verification）。

## Baselines / Comparisons

比较 POPE/CHAIR 类 object metrics、图像-文本一致性、region grounding、VQA accuracy 和人工评价；缓解方法按 training-based、inference-time、external knowledge 和 model architecture 分类。不同 metric 的覆盖范围不同，不能把 object hallucination 的分数直接当作总体视觉可靠性。

## Experiments / Findings

综述总结 LVLM 常被语言先验带偏：常见 object 更容易被幻觉生成，视觉细节弱、长尾关系和计数更难；扩大语言模型并不自动提高 grounding。有效路线通常需要视觉证据、负样本/对比训练、区域级监督与多维 evaluation。

## Ablation / Error Analysis

vision token 数、crop、prompt、temperature、图像质量、语言先验和 retrieval quality 都会影响结果；只测“图中是否有 object”会漏掉 relation/attribute。需要 text-only、vision-only、image corruption、region patching 和人工复核控制。

## Limitations

survey 文献中的定义、标注、模型和 metric 不统一；LVLM 迭代快，结论容易过时。降低 hallucination 还可能降低回答完整性或造成 over-refusal。

## 两句话总结

该 survey 将 LVLM hallucination 按对象、属性、关系、计数和 OCR 等细粒度类型组织，并总结原因、检测与缓解路线。它的主线是用视觉 grounding 和类型匹配的 benchmark 约束语言先验，而不是只追求一个总分。

## Evidence note

本笔记逐段核对 arXiv survey 的 taxonomy、benchmarks、metrics、mitigation 和 open questions。

