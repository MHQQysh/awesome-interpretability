# 013. FiDeLiS: Faithful Reasoning in Large Language Models for Knowledge Graph Question Answering

- **作者 / venue**：Yuan Sui, Yufei He, Nian Liu, X. He, Kun Wang, Bryan Hooi；Findings of ACL 2025
- **论文**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.436/)
- **任务**：KGQA；在知识图谱上寻找可验证、有效率的 LLM 推理路径

## 1. Introduction：为什么 KG 不自动带来 faithfulness

LLM 在复杂问题上可以生成流畅推理，但事实错误往往出现在中间步骤。把知识图谱接入模型看似可以解决问题，实际又有两个冲突：检索太宽会带来噪声和成本，检索太窄会漏掉真正的推理路径；agent 式逐步探索虽然灵活，却可能反复调用 LLM、生成不存在的边或在错误路径上继续推理。

FiDeLiS 的目标不是只提高答案 accuracy，而是让中间 reasoning path 能被 KG 中的实体、关系和逻辑规则逐步验证，并在效率上优于昂贵的全图搜索。

## 2. Method：Path-RAG + DVBS

### Path-RAG：先缩小候选空间

LLM 先从问题抽取关键实体/关系，并把它们转成 embedding；Path-RAG 迭代检索与 query 相关的实体和关系，形成候选 reasoning steps。它不是把所有三元组拼成 prompt，而是尽量保留“实体-关系”结构，从而减少噪声和 token 消耗。

### Deductive-Verification Beam Search（DVBS）

DVBS 在候选路径上做 step-wise beam search。每扩展一个 reasoning step，就检查它是否能由当前 KG 信息和已有路径演绎得到；同时使用局部/全局验证分数控制 beam。这样终点选择不仅依赖 LLM 的最大概率，还要考虑路径是否存在、是否连贯、是否满足逻辑约束。

## 3. Baseline 与对比

- **LLM-only**：Few-shot、CoT 等，代表没有外部 KG 结构约束的推理。
- **KG-enhanced**：ToG、RoG、CAF 等已有 KG 推理方法。
- **retriever 对比**：vanilla retriever、BM25、KAPING 风格检索与 Path-RAG。
- **backbone 对比**：GPT-3.5、GPT-4o、GPT-4-turbo、GPT-4o-mini 以及开源 LLM。
- **指标**：WebQSP、CWQ 的 Hits@1/F1，CR-LT 的准确率；同时报告 reasoning path validity、runtime、token usage 和 LLM 调用次数。

这些对比很关键：FiDeLiS 的 claim 是“faithful 且高效”，因此必须同时和 KG 方法比正确性、和简单 retriever 比效率、和 LLM-only 比是否真正减少幻觉。

## 4. Findings 与数值结果

- 在 GPT-3.5 设置下，FiDeLiS 在 WebQSP/CWQ/CR-LT 上达到 **79.32 / 63.12 / 67.34** 的核心结果；论文报告其整体优于 LLM-only 和强 KG baseline。
- 论文引用的强 baseline RoG 在部分设置下达到 **83.15 Hits@1、69.81 F1**，因此 FiDeLiS 的优势不是每个单项都绝对领先，而是准确率、路径有效性和推理效率的综合权衡。
- Path-RAG 显著优于简单文本化三元组检索；去掉 keywords 后，表中结果出现约 **3.83、4.11、5.65** 个百分点的下降，说明 query/entity/relation 的结构化检索不是装饰组件。
- DVBS 的 deductive verification 能提高终止信号和路径有效性；去掉 DVBS 或 beam search 组件后，准确率和 reasoning path validity 均下降。
- 运行分析显示 FiDeLiS 的 token 和调用成本低于一些 agent 式基线，但 beam width、搜索深度和验证频率会改变效率-准确率折中。

## 5. Ablation 与错误分析

- **w/o Path-RAG**：用 vanilla retriever 代替结构化候选生成，路径连接性和最终 QA 下降。
- **w/o DVBS**：直接由 LLM 选路径，容易保留格式正确但 KG 中不存在的边。
- **w/o keywords / embedding 变化**：检索候选召回变差，说明 Path-RAG 依赖实体和关系提示的质量。
- **path error analysis**：论文区分 format error、non-existent edge、wrong relation 和终点选择错误；这比只看答案错更能说明系统的 faithfulness 来源。

**一句话评价**：FiDeLiS 的可解释性来自“每一步都可回到 KG 验证”，而不是让 LLM 多写一段 rationale；它把 reasoning path 变成了结构化、可检查的中间对象。
