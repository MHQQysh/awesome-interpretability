# 414. Siren's Song in the AI Ocean: A Survey on Hallucination in Large Language Models

- **Authors:** Yue Zhang, Yafu Li, Leyang Cui, Deng Cai, Lemao Liu, Tingchen Fu, Xinting Huang, Enbo Zhao, Yu Zhang, Yulong Chen, Chen Xu, Longyue Wang, Anh Tuan Luu, Wei Bi, Freda Shi, Shuming Shi
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2309.01219
- **Tags:** `survey` `hallucination` `factuality` `evaluation` `mitigation`

## Introduction
LLM hallucination 指生成流畅但与事实、输入或任务约束不一致的内容，是部署中最重要的 trustworthiness 风险之一。本文从来源、类型、检测与缓解方法梳理领域发展，强调 hallucination 不能只用一个 factuality score 概括。

## Method / Framework
survey 将 hallucination 按来源和表现分类：知识/事实不正确、输入不一致、上下文/逻辑不一致、生成内容与用户意图不一致；再按 model pretraining、fine-tuning、decoding、外部知识和 evaluation 链条整理方法。

检测方法包括 reference-based/faithfulness metrics、fact checking、QA consistency、NLI、retrieval evidence、human evaluation 和 uncertainty；缓解包括数据清洗、instruction/RLHF、retrieval augmentation、constrained decoding、self-consistency、post-editing 和拒答。

## Baselines / Comparisons
比较 closed-book generation、RAG、external verifier、human evaluation、传统 NLG metrics 和 LLM-as-judge。每个基线覆盖不同错误：ROUGE 可能奖励表面重叠，NLI 可能受模型偏差影响，RAG 仍可能错误使用检索证据，self-consistency 可能重复共同错误。

## Experiments / Findings
作为 survey，文章没有统一新实验。主要 finding 是 hallucination 由数据、模型、训练目标、prompt、decoding 和知识边界共同决定；更大模型并不自动消除，开放式任务和长链推理更容易产生不受约束的错误。高质量缓解需要 detection、grounding 和 generation control 的组合。

## Ablation / Error Analysis
综述建议按 factuality、input consistency、citation correctness、拒答、长上下文和 adversarial prompt 分层测试，并比较有无 retrieval/verifier、不同 decoding、temperature 和 self-consistency。错误分析应区分知识缺失、检索失败、推理错误、引用错配和表达风格，避免把所有问题叫 hallucination。

## Limitations
不同论文的 hallucination 定义、数据集和评测协议差异很大，survey 无法给出统一排行榜。检测器本身可能 hallucinate，人工标注昂贵且领域依赖；闭源 LLM 的训练数据和内部置信度限制机制解释。

## 两句话总结
这篇早期 survey 将 LLM hallucination 按来源、类型、检测和缓解流程系统化，说明 factuality、输入一致性和引用忠实性必须分开测量。它的主线是从“生成后发现错误”走向 retrieval、verification、uncertainty 和约束生成的组合治理，而非单一修复。

## Evidence note
已读取 arXiv 2309.01219 本地 PDF 的 taxonomy、检测/缓解章节和开放问题；本文为 survey，没有虚构统一实验数字。
