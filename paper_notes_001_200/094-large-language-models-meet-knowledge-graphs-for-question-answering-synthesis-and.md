# 094. Large Language Models Meet Knowledge Graphs for Question Answering: Synthesis and Opportunities

> 人工精读笔记：EMNLP 2025 survey。重点记录 KG 在 LLM QA 中扮演的角色、评估维度和结构化知识的解释/验证边界。

## 0. 论文信息

- **作者**：Chuangtao Ma, Yongrui Chen, Tianxing Wu 等
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.1249)
- **主题**：LLM + knowledge graph、KGQA、RAG、复杂多跳问答

## 1. Introduction：要解决什么问题

LLM 擅长语言理解和开放生成，但参数知识可能过时、产生幻觉、难以解释中间推理；知识图谱提供实体、关系和结构化事实，却面临覆盖不全、检索复杂和动态更新问题。两者结合有潜力提升 factuality、multi-hop reasoning、可解释性和知识更新，但方法分散，KG 在系统中的角色也不统一。

论文的目标是给出一套综合视角：KG 是被当作 background knowledge 注入，还是 reasoning guideline，还是 output refiner/validator？不同角色如何影响检索、推理、答案质量、效率和公平性？

## 2. Method / Framework：怎么分类

### 2.1 KG as background knowledge

把 KG triples、subgraph 或 textualized paths 检索到 prompt/context，类似结构化 RAG。优点是向 LLM 提供外部事实并减少纯参数记忆；难点是实体链接、相关子图检索、图规模和 context length。

### 2.2 KG as reasoning guidelines

KG 不只是证据，还约束问题分解、logical form、path traversal 或 multi-hop reasoning。LLM agent 可以按关系路径逐步检索，或者用 KG 结构提示下一步，增强复杂问题的可追踪性。

### 2.3 KG as refiner/validator

先让 LLM 产生候选答案，再用 KG 过滤、重排、验证或生成事实摘要。这样可以检查答案是否满足实体类型和关系约束，但中间错误答案可能导致错误检索，KG 缺失或冲突也会误杀正确输出。

### 2.4 Hybrid/optimization

近期方法同时做检索、CoT/graph reasoning、reranking 和 output validation，并通过 index-based、prompt-based、cost-based 优化减少多次 LLM 调用。survey 将评估分为 Answer Quality、Retrieval Quality 和 Reasoning Quality。

## 3. Baseline / 对比维度

- **LLM-only QA**：没有 KG 的参数知识/普通 prompting，作为幻觉和可解释性基线。
- **标准 RAG**：dense/sparse 文档检索，比较结构化图信息的收益。
- **传统 KGQA**：logical form、模板或图查询，结构可解释但语言灵活性弱。
- **KG 角色对照**：background、reasoning guideline、refiner/validator、hybrid。
- **评测维度**：Answer Quality 不能替代 RetQ 和 ReaQ；一个答案正确可能是猜对，路径正确也可能无法生成最终答案。

## 4. Findings：综述得到的主要结论

KG 能通过类型约束、事实路径和验证提高 factual accuracy，并让答案有更清晰的 evidence chain；医学 KG reranking、KG-RAG、CoE/graph traversal 等方法在复杂 QA 中尤其有用。多跳任务中，KG 的拓扑结构比把 triples 当无序文本更重要，structure-aware retrieval 是主要发展方向。

但 KG 不是自动真值源。图谱可能不完整、过时或包含冲突，且中间答案错误时会产生 irrelevant retrieval；因此需要 source-aware validation、增量更新和不确定性估计。作者还指出，反复在每个 CoT step 调用 KG 会造成延迟和成本的二次增长。

在 explainability 方面，显式 subgraph/path 比纯生成 rationale 更容易审计；但图路径过大也会生成复杂、难读的解释。公平性也可能从 LLM 训练偏差转移到 KG 的缺失或偏置。

## 5. Ablation / 机制解释

论文综合的关键对照是把 KG 仅用于 context、用于推理约束、用于答案验证以及同时承担多角色。这样可以区分“知识加入提高 recall”与“结构/验证真正改变推理”的作用。综述不声称某一方法在所有 benchmark 上绝对最好，而是强调需要同时报告 answer、retrieval、reasoning 和 cost。

## 6. Limitations / 局限与复现注意

- survey 涵盖面广，但不同方法的 benchmark、KG、LLM 和指标不统一，无法形成严格的总排名。
- KG 的覆盖、时间版本、实体链接和冲突处理会强烈影响复现。
- 结构化路径可解释不代表 LLM 内部真正依赖该路径，仍需 attribution/causal validation。
- 长上下文、多轮 QA、动态图谱和实时更新的成本尚未解决。

## 7. 两句话总结

这篇 survey 讨论 LLM 与知识图谱如何结合来提升知识密集型 QA 的事实性、复杂推理和解释性。它将 KG 角色分为背景知识、推理指南、答案细化/验证和混合方案，指出最大瓶颈是结构感知检索、知识冲突/更新、推理成本以及尚未标准化的 reasoning quality 评估。
