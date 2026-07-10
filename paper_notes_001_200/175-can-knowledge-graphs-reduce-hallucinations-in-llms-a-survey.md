# 175. Can Knowledge Graphs Reduce Hallucinations in LLMs? A Survey

- **Authors:** see paper
- **Venue:** NAACL 2024
- **Paper:** https://aclanthology.org/2024.naacl-long.219
- **Tags:** Survey, Knowledge Graph, Hallucination, RAG

## Introduction

LLM hallucination 部分来自参数知识过时、实体关系混淆和缺少可验证 grounding。知识图谱提供结构化实体、关系和 provenance，但如何与 retrieval、prompt、generation 和 verification 结合，尚缺统一总结。

## Method / Framework

论文按 KG 的作用分类：作为检索源、prompt context、结构化 reasoning scaffold、constraint / verifier、以及训练和 fine-tuning data。还区分 intrinsic / extrinsic hallucination、KG construction、entity linking、graph completion、multi-hop retrieval 和答案验证。

## Baselines / Comparisons

比较 closed-book LLM、文本 RAG、KG-RAG、graph neural / symbolic reasoning、hybrid text+KG 和 post-generation fact checking。关注 factuality、coverage、multi-hop accuracy、解释 / provenance、延迟和 KG construction cost。

## Experiments / Findings

- KG grounding 通常降低实体和关系幻觉，尤其适合多跳结构事实；但 KG 不完整或过时会把错误结构强行注入答案。
- 纯 KG context 对开放域长文本和新事件覆盖不足，文本检索与 KG 混合更稳。
- 图结构提供可追踪路径和更好的 rationale，但 LLM 仍可能忽略 graph constraint 或把不存在的边生成出来。
- 主要瓶颈是 entity linking、图更新、复杂自然语言映射、context window 与图检索效率。

## Ablation / Error Analysis

调查了 KG size、hop 数、retriever、prompt serialization、graph verifier、text+KG 融合和 post-hoc checking。图太大增加噪声，图太小漏证据；错误常来自错误实体链接、缺失边、关系方向和过时事实。

## Limitations

综述跨论文的 benchmark、模型和 hallucination 定义不一致，不能直接横向排名。KG 的质量和领域覆盖往往比生成器更重要，实际构建成本可能抵消推理收益。未来需要统一、动态、带 provenance 的评测。

## 两句话总结

知识图谱能为 LLM 提供实体关系和可审计路径，从而降低一部分 hallucination，但它不是万能的事实数据库。图完整性、链接、更新和 LLM 是否真正遵守图约束决定最终效果。
