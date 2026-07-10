# 310. Hallucination of Multimodal Large Language Models: A Survey

- **Authors:** Zechen Bai, Pichao Wang, Tianjun Xiao, Tong He, Zongbo Han, Zheng Zhang, Mike Zheng Shou
- **Venue / year:** 2024/2025 survey
- **Paper:** https://arxiv.org/abs/2404.18930
- **Tags:** MLLM Hallucination, LVLM, Evaluation, Mitigation

## Introduction

MLLM/LVLM 不仅会生成一般文本 hallucination，还会出现与图像不一致的 object、attribute、relation、counting、OCR 和 grounding 错误。本文要建立 multimodal hallucination 的分类与研究地图，说明错误来自视觉编码、跨模态对齐、语言先验还是解码/训练。

## Method / Framework

survey 按 cause/taxonomy、detection/evaluation benchmarks、metrics 和 mitigation 组织 MLLM 文献。它覆盖 object hallucination、attribute/relation、faithfulness/visual grounding、image-text conflict、benchmark 与 automatic/human evaluation，并整理 training alignment、visual instruction tuning、retrieval、contrastive/decoding、post-hoc verification 等缓解路线。

## Baselines / Comparisons

比较不同 MLLM hallucination benchmark、CHAIR/POPE 类 object metrics、细粒度 relation/attribute metrics、human evaluation 与 model-judge；mitigation 按 training-based、prompt/inference-time、external retrieval/tool、vision module modification 分类。survey 强调指标对 hallucination 类型敏感，不能跨类别直接排序。

## Experiments / Findings

综述的共识是强语言先验可能覆盖弱视觉 evidence，模型即使看到图像也会生成常见但不存在的对象；更好的 VLM backbone 不自动解决 relation/counting/OCR。有效路线往往需要 visual evidence grounding、contrastive negative examples、区域/对象级评估和生成后验证，同时权衡 helpfulness、fluency、latency 与 over-refusal。

## Ablation / Error Analysis

图像质量、crop/region、prompt、视觉 token 数、temperature、语言先验和 retrieval quality 都会改变 hallucination；只测 object presence 会漏掉属性/关系错误，人工评价又有主观偏差。需要 image corruption、text-only/vision-only controls、region patching 和跨数据集/跨模型复现。

## Limitations

survey 所覆盖 benchmark 的定义、标注和 judge 不统一，MLLM 快速迭代使结果容易过时；多模态 hallucination 的 causal mechanism 仍比文本模型更难定位。减少 hallucination 还可能降低描述完整性或导致拒答。

## 两句话总结

该 survey 将 MLLM hallucination 从 object 错报扩展到 attribute、relation、counting、OCR 和跨模态 faithfulness，并整理检测/缓解方法。它的主线是用视觉证据和细粒度基准约束语言先验，同时承认不同 hallucination 类型需要不同指标与干预。

## Evidence note

本笔记逐段核对 arXiv survey 的 taxonomy、benchmarks/metrics、mitigation categories、open challenges 与 conclusion。

