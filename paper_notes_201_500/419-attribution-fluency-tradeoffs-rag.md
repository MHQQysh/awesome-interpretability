# 419. Characterizing Attribution and Fluency Tradeoffs for Retrieval-Augmented Large Language Models

- **Authors:** Renat Aksitov, Chung-Ching Chang, David Reitter, Siamak Shakeri, Yun-Hsuan Sung
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2302.05578
- **Tags:** `RAG` `attribution` `fluency` `factuality` `tradeoff`

## Introduction
RAG 通过检索证据提高 factuality 和 attribution，但加入更多或更长 context 可能让回答不流畅、重复或偏离对话。本文系统研究 response fluency 与 evidence attribution 的 global/local tradeoff，而不是只报告一个 factuality 分数。

## Method / Framework
使用 QReCC conversational question rewriting 的 decontextualized 数据，比较不同模型规模、temperature、prompt、证据数量和 evidence selection。fluency 用 sensibleness/specificity 类 auto-metric，attribution 用 NLI/证据支持度与人工 pilot 评估；作者区分跨模型的 **global tradeoff** 与固定模型/context budget 内的 **local tradeoff**。

## Baselines / Comparisons
比较 top-1 vs top-k retrieval、gold/oracle evidence、不同 PaLM 模型规模、temperature、dialog/history context 和 reranking 策略。指标包括 fluency、attribution 以及综合 F1，避免只优化一个维度。

## Experiments / Findings
更大模型通常同时提高 fluency 和 attribution；naive top-k retrieval 提高 evidence recall/attribution，却常损伤 fluency。context 中 dialog/evidence 比例是局部 tradeoff 的关键约束；没有固定 context budget，很多 prompt 差异只是 global movement，不能直接归因于 retrieval choice。

通过 input-level reranking/NLI selection，作者能在相同约束下提高 attribution 并部分恢复 fluency；小模型配合 reranking 的 F1 可以接近更大模型/Oracle evidence 的水平。核心不是消灭 tradeoff，而是识别约束后把 Pareto front 往外推。

## Ablation / Error Analysis
top-k 数量、context ratio、temperature、model size、prompt structure 和 reranking criterion 是主要消融。证据过多会稀释相关信息、增加冲突和重复；top-1 可能缺少关键 passage，top-k 则带来噪声。auto fluency/attribution 与 human preference 也存在偏差。

## Limitations
实验使用 100 个过滤后的 QReCC dev examples 和特定 PaLM/metric，不能代表所有 RAG。NLI attribution 只是 proxy，证据支持不等于回答逻辑正确；global/local tradeoff 定义依赖 context budget 和目标指标。

## 两句话总结
本文显示 RAG 的 top-k retrieval 往往提高证据归因却牺牲 fluency，且真正的 local tradeoff 只有在 context ratio 等约束固定后才可测量。通过证据 reranking 和综合 F1，可以在 attribution 与可读性之间取得更好的 Pareto 折中，但不能把更多 context 当成无条件改进。

## Evidence note
已读取 arXiv 2302.05578 v2 本地 PDF 的 QReCC 设定、global/local tradeoff、top-k retrieval、reranking 和结论。
