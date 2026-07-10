# 324. Unifying Corroborative and Contributive Attributions in Large Language Models

- **Authors:** Theodora Worledge, Judy Hanwen Shen, Nicole Meister, Caleb Winston, Carlos Guestrin
- **Venue / year:** 2023/2024 preprint
- **Paper:** https://arxiv.org/abs/2311.12233
- **Tags:** Attribution, Citation, Training Data, Verifiability

## Introduction

LLM attribution 这个词混合了两种不同问题：corroborative attribution 是回答中的 claim 是否有外部 source 支持，contributive attribution 是哪些训练样本/数据对模型输出产生影响。法律、医疗和科研应用可能同时需要两者，单一 citation 或 data attribution 都不够。

## Method / Framework

论文提出统一 attribution framework，把输出 claim、外部 evidence、模型内部/训练数据影响和用户验证连接起来，分析不同 attribution methods 如何落入同一抽象。框架以 use case 为中心，区分“证明答案正确”与“解释模型为何产生答案”，并讨论 citation generation、retrieval/source attribution、influence functions、data tracing 等组件。

## Baselines / Comparisons

比较 corroborative/citation methods、contributive/training-data attribution methods 及只提供其中一种的系统；评价维度包括 completeness、relevance、faithfulness、verifiability、cost 和隐私。论文是 SoK/framework，不是单一新模型 benchmark。

## Experiments / Findings

核心 finding 是 attribution 的目标必须先由 use case 明确：医疗问答要知道 claim 的外部证据，同时可能要追踪训练数据来源；法律文本要审计引用和责任来源。两类 attribution 在概念上可统一，但技术实现、评价集和风险不同，不能用 citation completeness 代替 training influence，也不能反过来。

## Ablation / Error Analysis

只提供 citation 会出现 source irrelevant/fabricated，只有 training attribution 又不能证明当前 claim 正确；claim segmentation、evidence retrieval、model version 和 data access 都会改变结果。需要分别做 source entailment、influence faithfulness 和 user verification，并报告失败类型。

## Limitations

统一框架没有解决训练数据不可访问、版权隐私、长输出 claim alignment 和 influence 的非唯一性；不同方法的 attribution semantics 仍可能不一致。论文提供研究路线而非可直接部署的全能 attribution tool。

## 两句话总结

文章把“支持答案的外部引用”和“造成答案的训练数据影响”统一放进 LLM attribution 框架，同时保留两者不同的验证目标。它的核心贡献是避免把引用、数据归因和解释混为一谈，为医疗/法律等场景设计组合式 attribution 提供原则。

## Evidence note

本笔记核对 arXiv 摘要、framework/use-case discussion 和 attribution taxonomy；未将 SoK 观点改写成新实验数字。

