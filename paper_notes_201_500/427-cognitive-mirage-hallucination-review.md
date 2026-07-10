# 427. Cognitive Mirage: A Review of Hallucinations in Large Language Models

- **Authors:** Hongbin Ye, Tong Liu, Aijia Zhang, Wei Hua, Weiqiang Jia
- **Venue / year:** review, 2023
- **Paper:** https://arxiv.org/abs/2309.06794
- **Tags:** `survey` `hallucination` `causes` `detection` `mitigation`

## Introduction
“Cognitive mirage”强调 LLM 的语言流畅性会让用户把统计生成误认为可靠认知。文章回顾 hallucination 的定义、成因、类型、检测与缓解，重点讨论模型知识与人类信任之间的错位。

## Method / Framework
综述按 data/model/training/generation/user interaction 分析来源，按 factuality、consistency、knowledge boundary 和 context grounding 分类，再整理 retrieval、fact-checking、prompting、fine-tuning、uncertainty 和 human oversight。

## Baselines / Comparisons
对比传统 NLG metrics、human evaluation、external knowledge、self-consistency、RAG 和 post-hoc verifier。不同基线的评价对象不同：流畅性、事实、输入一致性和用户信任不能互相替代。

## Experiments / Findings
文章为 review，主要结论是 hallucination 是系统性风险，来自训练语料噪声、知识缺失、目标函数、decoding 和交互提示；检测和缓解需要结合模型、数据、知识源和用户流程。

## Ablation / Error Analysis
综述建议按 hallucination type、domain、temperature、retrieval context 和模型规模分层；错误分析要区别“事实不对”“证据不支持”“任务误解”和“过度拒答”。

## Limitations
综述缺少统一 benchmark 和新实证，且标题/版本元数据在不同索引中不一致。结论应作为领域地图，不应当作某个算法的性能证明。

## 两句话总结
这篇 review 将 hallucination 解释为 LLM 流畅输出造成的 cognitive mirage，梳理来源、检测、缓解和人类信任风险。它强调 factuality 与用户依赖需要联合治理，但没有统一实验算法。

## Evidence note
已读取本地 review PDF 的 taxonomy/causes/detection/mitigation 章节；README 对该条目的 arXiv metadata 不完整，保留标题证据边界。
