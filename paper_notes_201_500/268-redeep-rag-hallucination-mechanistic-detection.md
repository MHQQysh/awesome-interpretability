# 268. ReDeEP: Detecting Hallucination in Retrieval-Augmented Generation via Mechanistic Interpretability

- **Authors:** Zhongxiang Sun, Xiaoxue Zang, Kai Zheng, Yang Song, Jun Xu, Xiao Zhang, Weijie Yu, Yang Song, et al.
- **Venue / year:** ICLR 2025
- **Paper:** https://arxiv.org/abs/2410.11414
- **Tags:** RAG Hallucination, Mechanistic Interpretability, Copying Heads, Knowledge FFNs

## Introduction

RAG 提供正确 external context 仍可能 hallucinate：模型一方面没有正确使用检索文本，另一方面可能把 parametric memory 强行注入回答。只看 perplexity、entropy 或 response/context 相似度会把这两种来源混在一起。ReDeEP 的问题是能否用 mechanistic signals 解耦 external-context use 与 parametric-knowledge injection，再检测并缓解 RAG hallucination。

## Method / Framework

作者定义 External Context Score (ECS)，用 attention heads 衡量 response 对 retrieved context 的利用；定义 Parametric Knowledge Score (PKS)，用 FFN contribution 衡量模型从参数记忆加入 residual stream 的程度。经验和 causal intervention 指向两类组件：Copying Heads 应复制外部证据，但可能丢失已 attend 的信息；later-layer Knowledge FFNs 可能过度注入参数知识。ReDeEP 将两者作为 covariates 回归 hallucination：PKS 越高、ECS 越低，风险分数越大；token-level 逐 token 计算，chunk-level 对 context/response 分块后用 embedding 与 mean-pooled attention 提高效率。AARF (Add Attention, Reduce FFN) 增强 Copying Heads、抑制 Knowledge FFNs。

## Baselines / Comparisons

实验在 RAGTruth 和 Dolly (AC) 上，用 LLaMA2-7B/13B、LLaMA3-8B 比较 SelfCheckGPT、Perplexity、LN-Entropy、Energy、Focus、Prompt、LMvLM、ChainPoll、RAGAS、Trulens、RefCheck、P(True)、EigenScore、SEP、SAPLMA 和 ITI。指标包含 AUC、PCC、Recall、F1；还用 attention/FFN intervention 与 control group 验证机制，而非只看 detector 结果。

## Experiments / Findings

- LLaMA2-7B 在 RAGTruth 上，ReDeEP-token AUC/PCC/Recall/F1 为 0.7325/0.3979/0.6770/0.6986，ReDeEP-chunk 为 0.7458/0.4203/0.8097/0.7190；Dolly 上 chunk 为 0.7949/0.5136/0.8245/0.7833。
- 1024 个 attention heads 中 1006 个在 truthful 数据上的 ECS 高于 hallucination；77.5% 的数据中 heads 已 attend correct information，低 ECS 更常来自后续生成阶段丢失 copying 信息，而非最初没有看对 context。
- 后层 FFN 的 PKS 在 hallucination 上更高；干预 Copying Heads/Knowledge FFNs 对 NLL 的影响显著强于 control。结果支持“外部证据被遗忘 + 参数知识过度注入”是两条不同但可叠加的路径。

## Ablation / Error Analysis

ReDeEP 的消融分别只用 PKS、只用 ECS、二者 full：LLaMA2-7B RAGTruth token AUC 为 0.6950/0.7234/0.7325，chunk 为 0.6180/0.7098/0.7458；Dolly chunk 为 0.6383/0.7552/0.7949。说明两个 covariate 互补。chunk-level 提升 recall/F1 但会损失 token-level 定位精度；ECS/PKS 是 architecture-specific proxy，copy-suppression heads、层选择和 chunk 划分也会影响结果。

## Limitations

实验集中在 LLaMA 系列与“external context 正确”的 RAG setting，不能覆盖检索错误、上下文冲突、长文档和工具调用。AARF 的 reweighting 可能提升 truthfulness 却损伤模型原有能力，且 hallucination label 与 mechanistic score 的因果方向仍依赖 intervention design。机制解释揭示相关 circuit，但不代表每个模型都共享同样 heads/FFNs。

## 两句话总结

ReDeEP 用 attention 的 ECS 和 FFN 的 PKS 解耦“是否使用外部证据”和“是否过度调用参数记忆”，再通过回归检测 RAG hallucination。其关键发现是很多 head 已经看到了正确 context，但信息会在后层丢失，同时 Knowledge FFNs 过度写入参数知识，二者可用 AARF 进行干预。

## Evidence note

本笔记逐段核对 ICLR/arXiv PDF 的 causal framing、Sections 3-5、Tables 1-3 和 intervention appendix；数值来自正文表格。

