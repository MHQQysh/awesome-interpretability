# 095. LLMs as a Synthesis between Symbolic and Distributed Approaches to Language

> 人工精读笔记：Findings of EMNLP 2025 position paper。本文以可解释性文献为证据，重新讨论 LLM 是否只属于 distributed camp。

## 0. 论文信息

- **作者**：Gemma Boleda
- **来源**：[Findings of EMNLP 2025](https://aclanthology.org/2025.findings-emnlp.498)
- **类型**：position paper / interpretability synthesis

## 1. Introduction：要解决什么问题

语言研究长期在 symbolic/discrete/rule-based 与 distributed/continuous/fuzzy 两种传统之间争论。深度学习成功常被解读为 distributed approach 获胜，但作者认为这是二分法误读：Transformer 的向量表示虽然连续，却可以通过 attention、MLP 和组合 circuit 实现接近符号的局部操作；同时又保留模糊、分布式和上下文敏感的能力。

论文要解决的问题是怎样把近年的 mechanistic interpretability 证据放回语言理论：LLM 是否能在不同任务和不同层级之间灵活切换 symbolic-like 与 distributed processing？答案不是把 LLM 还原成传统语法，而是把它视为两者的 synthesis。

## 2. Method / Evidence framework

作者不是提出训练算法，而是按 representation 和 processing 两条线综述案例。近符号证据包括可定位的 syntax/agreement feature、copying/induction heads、词法/形态类别和局部 circuit；分布式证据包括 polysemanticity、superposition、词义梯度和非组合的多词表达。

论文把“symbolic”放宽为 near-symbolic：不是要求一个 neuron 完全等于一个词法类别，而是要求少量方向/组件能被稳定解释、具有离散边界或实现可复用规则。这样可以讨论 neural circuit 的算法功能，而不把连续参数误称为传统符号。

## 3. Baseline / 对比维度

- **纯 symbolic grammar**：可解释、离散、规则明确，但难以覆盖模糊类别、例外和大规模语义。
- **纯 distributed connectionism**：能从数据学习连续统计模式，但内部单元常难解释。
- **LLM mechanistic circuits**：作为 synthesis case，具有连续向量实现与 near-symbolic component/function 并存的特征。
- **表示 vs 处理**：可解释 feature 不等于可解释算法；需要同时看局部 representation、组件连接和行为干预。

## 4. Findings：论文的主要论证

### 4.1 近符号形态句法

作者回顾了 agreement、part-of-speech、morphosyntax 和语言结构研究：某些概念可以被少量方向、neuron 或 attention circuit 解码，模型在结构任务中表现出近似离散的类别边界。这说明“向量”不排斥 rule-like processing。

### 4.2 Attention/MLP 的算法化功能

copying、induction、memory retrieval、FFN vocabulary promotion 等机制把信息沿可追踪路径传递；它们不等于手写规则，但在具体上下文中执行类似复制、匹配、类别提升和组合的操作。这是 LLM 具有 near-symbolic processing 的核心证据。

### 4.3 Distributed 与 fuzzy 仍然不可替代

词义、polysemy、multi-word expression 和 form-function mismatch 往往没有清晰边界；模型需要在分布式空间中表达相似性、上下文依赖和渐变性。superposition 不是纯缺陷，也让有限维度承载更多特征。

### 4.4 灵活切换是能力来源

作者的综合解释是：LLM 在需要类别/结构时会形成较集中的 near-symbolic subspace，在需要相似性、语境和模糊语义时使用 distributed representation，二者并非互斥。可解释性研究应避免“找到一个方向就宣布模型是符号系统”，也避免“连续向量就一定没有符号”。

## 5. Ablation / 比较解读

这是 position paper，没有统一实验消融；其证据对照是跨论文的 component localization、causal intervention、probe、circuit analysis 与自然语言任务行为。作者明确提醒：很多现有结果只显示相关性或可解码性，尚不足以证明完整 symbolic rule，也需要区分 interpretable 与 symbolic。

## 6. Limitations / 局限与复现注意

- 综述案例主要来自英语及少量语言现象，不能代表所有语言结构。
- near-symbolic 的定义比传统符号宽，存在解释边界和术语主观性。
- 许多 circuit 结论依赖特定模型、层和 prompt，是否能规模化泛化仍未知。
- 该文提供理论综合而非新的统一 benchmark，不能直接产生方法性能排名。

## 7. 两句话总结

论文主张 LLM 不是 distributed-only 系统，而是能同时使用连续分布式表示和近符号的局部表示/处理。它通过 mechanistic interpretability 中的 feature、attention circuit、agreement 与 polysemanticity 证据论证这种灵活性可能正是 LLM 语言能力强的原因，但 near-symbolic 仍不等于完整传统符号系统。
