# 424. A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions

- **Authors:** Lei Huang, Weijiang Yu, Weitao Ma, Wei-Hong Zhong, Zhangyin Feng, Haotian Wang, Qianglong Chen, et al.
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2311.05232
- **Tags:** `survey` `hallucination` `taxonomy` `evaluation` `mitigation`

## Introduction
hallucination 是 LLM 规模化应用的核心障碍：模型输出语法和风格正确，却不被输入、外部事实或上下文支持。本文从 principles、taxonomy、detection、evaluation、mitigation 和 open questions 建立较完整的研究地图。

## Method / Framework
综述按 hallucination 的成因与表现分类，覆盖 intrinsic/extrinsic、事实性/忠实性、知识边界、训练数据、decoding、prompt 和 alignment；再整理 retrieval、verification、self-reflection、uncertainty、data/model editing、RLHF 和 decoding control 等路线。

## Baselines / Comparisons
比较 reference-based metrics、NLI/QA consistency、human evaluation、knowledge-grounded verification、RAG 和 self-consistency。文章强调不同任务的 hallucination 定义不同，摘要事实错误、对话不一致和多模态幻觉不能共用一个分数。

## Experiments / Findings
survey 没有统一新实验。领域流程从发现 hallucination 类型，发展到自动 detection benchmark，再到 retrieval/verification 和 training-time mitigation；当前难题是开放世界 factuality、长文本、多语言、多模态、动态知识和评测器自身可靠性。

## Ablation / Error Analysis
建议按来源、任务、知识范围、temperature、prompt、retrieval evidence 和 model scale 分层；错误分析区分事实错、证据错配、推理跳步、输入不一致和安全拒答。单纯降低 hallucination rate 可能通过过度拒答实现，必须同时报告 utility。

## Limitations
综述覆盖广但文献协议不统一，很多 detection/mitigation 方法依赖额外模型或昂贵人工标注。taxonomy 会随模型与多模态发展变化；不应把 survey 中引用结果直接当成统一实验比较。

## 两句话总结
本文把 LLM hallucination 按原则、类型、检测、评估和缓解路线系统整理，强调不同错误机制需要不同验证。它的开放问题集中在可泛化 factuality、动态知识、长链推理、跨模态和“降低错误是否只是增加拒答”。

## Evidence note
已读取 arXiv 2311.05232 v2 本地 PDF 的 taxonomy、detection/mitigation 与 open questions；本文为 survey。
