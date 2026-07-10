# 251. G-Refer: Graph Retrieval-Augmented Large Language Model for Explainable Recommendation

- **Authors:** Yuhan Li, Xinni Zhang, Linhao Luo, Heng Chang, Yuxiang Ren, Irwin King, Jia Li
- **Venue / year:** WWW 2025
- **Paper:** https://arxiv.org/abs/2502.12586
- **Tags:** Explainable Recommendation, GraphRAG, Collaborative Filtering, LLM

## Introduction

解释推荐需要同时回答“推荐什么”和“为什么推荐”。既有方法往往把用户-物品交互图中的协同过滤信号压缩成隐向量，再交给语言模型；问题是图结构复杂，隐向量与自然语言之间存在 modality gap，而且检索到的邻居可能包含大量噪声。G-Refer 的核心问题是：能否从图中检索出明确、可读、与当前用户-物品对真正相关的 CF 证据，并让 LLM 基于这些证据生成稳定的解释。

## Method / Framework

框架分三步。第一步是 hybrid graph retrieval：node-level retriever 从用户/物品节点的结构与语义信息检索证据，path-level retriever 从用户-物品交互图中检索多跳路径；例如“用户看过 Iron Man，另一个用户也看过它并喜欢 Doctor Strange”提供结构性证据，而共享演员提供语义性证据。第二步是 graph translation，把节点、路径和 profile 翻译成面向人的文本，让检索结果不再只是不可读的 embedding。第三步是 knowledge pruning 与 retrieval-augmented fine-tuning (RAFT)：先删除低相关/冗余训练样本，再用 LoRA 微调 LLaMA，使模型学会读取检索文本并生成解释。

## Baselines / Comparisons

作者在 Amazon-books、Yelp、Google-reviews 三个数据集上比较 NRT、Att2Seq、PETER、PEPLER 和 XRec，覆盖 GRU/LSTM/GPT/LLaMA 风格的解释推荐模型。指标不是单一的 BLEU，而是 GPTScore、BERTScore 的 Precision/Recall/F1、BARTScore、BLEURT 和 USR；同时报告这些指标的标准差衡量稳定性。主实验使用 LLaMA 2-7B，并额外报告 LLaMA 3-8B；检索路径/节点数量为 2，最大路径长度 5，pruning ratio 为 70%。

## Experiments / Findings

- G-Refer 在三个数据集上都提高解释质量并降低波动。以 LLaMA 3-8B 为例，Amazon-books 的 GPTScore 为 82.82、BERT F1 为 0.4289；Yelp 为 75.16/0.4003；Google-reviews 为 71.73/0.4592。相较 XRec，Google-reviews 的 BERT Recall 从 0.4069 提升到 0.4935，说明检索到的 CF 证据更完整。
- 节点与路径证据承担不同作用：Yelp 示例主要靠 node-level 证据形成共享属性解释，Google-review 示例则主要依靠 path-level 证据串起用户、物品和相似用户的交互链。
- 模型规模存在收益，但不是唯一因素：Qwen 0.5B/1.5B 明显较弱，Qwen 3B 已接近 7B，表明检索文本和 RAFT 能把一部分图推理负担从大模型转移到显式上下文中。

## Ablation / Error Analysis

Yelp 上去掉 path-level retrieval 后 BERT F1 为 0.3927，去掉 node-level 为 0.3966，去掉完整 GraphRAG 为 0.3880，而完整模型为 0.4003；Google-reviews 上对应结果为 0.4560、0.4544、0.4468 和 0.4592。去掉 pruning 的结果几乎接近完整模型，说明 pruning 主要改善训练效率和噪声控制，而不是单独创造全部精度收益。错误主要来自实体/路径检索不准、图覆盖不足和生成模型把 profile 当成事实证据；因此“解释可读”不等于“解释忠实”。

## Limitations

方法依赖用户-物品图的覆盖率、图中 profile 的质量和实体语义编码；冷启动用户、稀疏图与新物品会削弱检索。RAFT 仍需要微调，且 GPTScore、BERTScore 等自动指标无法完全判定解释是否真的支持推荐。作者将 training-free retriever 和跨任务迁移留作未来工作。

## 两句话总结

G-Refer 用结构/语义混合图检索把协同过滤信号翻译成可读文本，再通过 pruning 与 RAFT 让 LLM 生成解释。它的关键发现是显式图证据同时提升了解释质量和稳定性，但路径正确性、图覆盖和指标忠实性仍是部署瓶颈。

## Evidence note

本笔记逐段核对 arXiv/W3C 版本 PDF 的 Introduction、Methodology、Experiments、Table 1/2 和 Conclusion；数值来自正文表格。

