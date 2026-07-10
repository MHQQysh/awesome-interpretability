# 099. Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation

> 人工精读笔记：EMNLP 2024。重点是 MIRAGE 如何从 RAG 模型内部找到真正影响生成 answer 的 context token，而不是只做外部 entailment。

## 0. 论文信息

- **作者**：Jirui Qi, Gabriel Stanovsky,等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.347)
- **方法**：MIRAGE（Model Internals-based RAG Explanations）
- **任务/数据**：XOR-AttriQA、ELI5；Zephyr beta、Llama 2 等开源 LLM

## 1. Introduction：要解决什么问题

RAG 系统需要说明生成答案由哪些 retrieved documents 支持。现有 self-citation 让模型自己插 citation，可能产生 plausible 但不忠实的引用；entailment-based attribution 用 NLI 判断答案与文档是否蕴含，成本高且不一定反映生成模型实际使用的 context。

MIRAGE 的目标是模型内部 attribution：如果某个答案 token 对 context token 的梯度/activation 敏感，就把该 token/span 归因到对应 document，再聚合成 sentence-level citations。这样解释尽量对齐模型真实生成过程，并允许比整段文档更细粒度的 token/span 选择。

## 2. Method / Framework：怎么解决

### 2.1 Context-sensitive token identification

对每个生成 answer token 做 Contextual Token Importance（CTI）分析，比较有 context 与无 context/被遮挡 context 时的输出变化，筛出受检索内容影响的 token。MIRAGE 只在这些 context-sensitive token 上继续 attribution，避免把所有 output token 都强行分配给文档。

### 2.2 Contrastive attribution

使用输入 embedding 的 gradient-based attribution，得到 answer token 对各 context token 的分数；再做 context-sensitive filtering（CCI/CTI）与阈值化，聚合成 document-level 或 sentence-level citations。MIRAGE CAL 用 calibration examples 选择阈值，MIRAGE EX 不依赖标注，通过 top-K/top-% 过滤。

## 3. Baseline / 对比基线

- **Self-citation**：LLM 自己在答案中插入文档编号，最直接但容易幻觉引用。
- **TRUE / entailment-based AA**：NLI 模型判断 answer-document entailment，代表外部语义支持。
- **Supervised answer attribution**：在有 annotation 的 XOR-AttriQA 上训练/校准。
- **MIRAGE CAL vs MIRAGE EX**：有校准示例与无校准版本。
- **Top 3 vs Top 5%/Top-%**：固定文档数与自适应阈值的选择对照。

## 4. Experiments / Findings：结果如何读

在 XOR-AttriQA 的人类 answer attribution 上，MIRAGE CAL 与人类标注的 agreement 约为 82.2/82.5/92.0 等多项指标，平均约 86.9；MIRAGE EX 无校准也达到约 79.0/74.1/90.8，平均约 82.7。结果说明仅用 model internals 就能接近或超过外部 entailment baseline，且跨语言和不同过滤方式较稳。

在 ELI5 长答案上，MIRAGE EX Top 5% 比 Top 3 通常更好，Zephyr beta 的 citation quality F1 约 45.6，Llama 2 约 27.6；它在长、复杂答案中优于 self-citation，原因是自适应选择能保留少量真正影响输出的 document/token，而不是固定引用三个文档。

定性案例显示 self-citation 会输出空 citation 或引用提到主题但没有支持答案的文档；MIRAGE 能找到具体支撑 span。反例也很重要：同名实体在多个文档出现时 attribution 可能同时选择多个来源，gradient sensitive 不等于人类认为的唯一充分证据。

## 5. Ablation / 机制解释

- 有/无 CCI/CTI filtering：验证先找 context-sensitive output token 是否改善 citation。
- MIRAGE CAL/EX：测标注校准与无标注阈值的 trade-off。
- Top 3/Top 5%/不同阈值：解释文档选择粒度与长答案适配。
- 不同 attribution function、语言、模型和 prompt：检查结果是否依赖单一 gradient 设定。

## 6. Limitations / 局限与复现注意

- Gradient attribution 可能受饱和、tokenization、prompt 和层选择影响，不是完整 causal proof。
- MIRAGE 在开源 LLM 上运行；更大闭源模型的内部状态不可访问。
- ELI5 的人工 AA 标注有限，很多定量结果依赖 TRUE/NLI 或 LLM 生成答案。
- 长上下文会增加 forward/backward 计算，且模型没有使用某文档不代表该文档在事实层面不支持答案。

## 7. 两句话总结

MIRAGE 解决 RAG self-citation 可能自说自话、NLI attribution 又不反映生成模型内部使用的问题。它先找 context-sensitive answer token，再用内部 gradient/contrastive attribution 聚合文档引用，在 XOR-AttriQA 和 ELI5 上优于或接近 entailment/self-citation baseline，但内部敏感性仍不等于充分的事实证明。
