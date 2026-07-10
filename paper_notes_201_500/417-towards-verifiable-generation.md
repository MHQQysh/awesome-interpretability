# 417. Towards Verifiable Generation: A Benchmark for Knowledge-aware Language Model Attribution

- **Authors:** Xinze Li, Yixin Cao, Liangming Pan, Yubo Ma, Aixin Sun
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2310.05634
- **Tags:** `knowledge-attribution` `verifiable-generation` `benchmark` `hallucination`

## Introduction
生成式 LLM 往往把多个事实、实体和关系混在一段回答中，传统 citation-level 指标无法判断每个 claim 是否有证据。本文提出 knowledge-aware attribution benchmark，目标是评价模型能否将生成文本中的知识声明 grounded 到结构化知识/证据，并在缺失知识时承认不知道。

## Method / Framework
工作构造 BioKaLMA 等 biography/knowledge-graph 数据，输入 general/specific question 和 minimum knowledge set，要求模型生成带知识子图 attribution 的回答。句子可映射到一个或多个知识子图，也可输出 `[NA]` 表示知识图谱中没有该知识。

评价拆成 claim generation、knowledge grounding、citation completeness、citation correctness 和 conscious incompetence。这样能区分“回答很流畅但没有支持”“引用了错误实体”“部分句子可验证、部分应标 NA”等错误。

## Baselines / Comparisons
比较普通 LLM generation、retrieval-augmented generation、citation prompting、知识图谱/证据约束方法及不同 attribution granularity。主指标不是单一 QA accuracy，而是 claim-level/knowledge-level precision、recall、F1 和 attribution consistency。

## Experiments / Findings
benchmark 显示验证式生成需要同时控制内容和证据：加入 citation 指令不保证引用正确，模型还会把相关但不支持的知识图谱片段贴到句子上。`[NA]` 机制可测量模型是否会诚实地表达 knowledge gap，而不是被迫编造。

## Ablation / Error Analysis
句子分割粒度、minimum knowledge set、general vs specific question、允许多图谱映射和 `[NA]` 是关键消融。错误来自多跳关系、实体消歧、部分支持和知识图谱缺失；若只按整段回答评分，这些局部错误会被平均掉。

## Limitations
知识图谱覆盖不等于开放世界事实，自动构造/匹配可能漏标合理证据。benchmark 主要评估外部 attribution，不证明内部 reasoning faithful；引用格式和可读性也可能影响用户实际信任。

## 两句话总结
本文用 claim-level 知识图谱 attribution、citation correctness 和 `[NA]` knowledge-gap 标记，把“生成答案”变成可验证的多层 benchmark。它的核心启示是引用必须支持具体句子，且诚实拒答与内容正确同样是可测的能力。

## Evidence note
已读取本地 PDF 的 BioKaLMA、minimum knowledge set、conscious incompetence 和 attribution benchmark 设计；未把知识图谱覆盖外推为现实世界真值。
