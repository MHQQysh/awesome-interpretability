# 425. A Survey of Hallucination in Large Foundation Models

- **Authors:** Vipula Rawte, Amit Sheth, Amitava Das
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2309.05922
- **Tags:** `survey` `foundation-model` `hallucination` `multimodal`

## Introduction
foundation models 不只在文本中 hallucinate，也会在视觉、语音、代码和跨模态生成中产生与输入或现实不一致的内容。本文将 hallucination 从 LLM 扩展到 large foundation models，关注统一定义、来源、检测和缓解。

## Method / Framework
survey 按模态和任务梳理 hallucination：text generation 的 factuality/faithfulness、vision-language 的 object/attribute/relationship errors、speech/vision generation 和跨模态 grounding。方法分为 data/training、model architecture、knowledge retrieval、decoding、post-hoc verification 和 human evaluation。

## Baselines / Comparisons
对比 closed-book foundation model、retrieval-grounded system、multimodal encoder-decoder、rule/metric verifier 和人工标注。评价包括 factual accuracy、input consistency、caption/answer faithfulness、human preference 和 hallucination detection recall。

## Experiments / Findings
作为 survey，论文没有统一新实验。主要 finding 是 hallucination 的根源既有知识缺失和语言模型先验，也有跨模态对齐、视觉 grounding、训练数据噪声和生成目标；因此 text-only 的修复不能直接应用于 multimodal foundation models。

## Ablation / Error Analysis
需要按模态、输入证据、retrieval、decoding、model scale 和 hallucination type 做消融。跨模态常见错误是图中不存在的 object、错误属性/关系、视觉证据和文本输出不一致；单纯 caption similarity 可能漏掉细粒度关系错。

## Limitations
不同 foundation models 的 modality、任务和 metric 很难统一，survey 时代较早，无法覆盖后续多模态 LLM。缺少统一 hallucination ground truth 和可解释机制；外部 verifier 也可能受模型/数据偏差影响。

## 两句话总结
本文将 hallucination 从语言扩展到视觉、语音和其他 foundation models，强调知识缺失、生成先验与跨模态 grounding 共同造成错误。它的核心路线是按模态拆分 error taxonomy，再结合数据、检索、架构、解码和验证，而不是用文本 factuality 一个指标解决全部问题。

## Evidence note
已读取 arXiv 2309.05922 本地 PDF 的 foundation-model taxonomy、跨模态错误和 mitigation survey；本文没有统一新 benchmark。
