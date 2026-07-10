# 471. Can Embeddings Analysis Explain Large Language Model Ranking?

- **Authors:** Claudio Lucchese, Giorgia Minello, Franco Maria Nardini, Salvatore Orlando, Raffaele Perego, Alberto Veneri
- **Venue / year:** CIKM 2023
- **Paper:** [DOI:10.1145/3583780.3615225](https://doi.org/10.1145/3583780.3615225)
- **Tags:** `neural-ir` `embedding-geometry` `ranking-explanation` `query-performance-prediction`

## Introduction
BERT/RoBERTa/ELECTRA cross-encoders 的 relevance score 很有效，却难以解释为什么 query-document pair 排在某处。论文不用自由文本 rationale 或 token attribution，而是跟踪 query/document token embedding 在层间如何移动、聚类和靠近，寻找 latent-space geometry 与最终 ranking 的关系。

## Method / Framework
作者分析 cross-encoder 的 query/document token representations，观察 token 在各层的调整、query-document 交互和聚类结构，并用 embedding arrangement 预测 relevance/错误。核心假设是 token 距离、聚类和 score evolution 能提供 ranking 的局部解释，也可用于 early exit 与 query performance prediction。

## Baselines / Comparisons
论文把 embedding analysis 与 IR axiomatic diagnosis、free-text explanation、feature attribution 作为三类解释路线对照；具体工作集中在神经 ranker 的 latent trajectory，而不是重新训练一个解释模型。公开全文中的重点是 cross-validation 的 BAcc/accuracy 与误排序案例。

## Experiments / Findings
实验观察到 relevance score 与 token embedding adjustment 存在稳定联系：相关 query/document 的 token 更容易在 latent space 聚类，层间表示变化可以暴露模型依赖的词对关系。作者据此认为 embedding distance/cluster 可帮助发现模型的 ranking pitfalls，并提出在较早层观察演化以降低推理成本。

## Ablation / Error Analysis
错误案例分析关注“模型分数/embedding arrangement 与正确性不一致”的 pair，而不是单纯把高分当解释。该工作没有像 causal patching 那样移除单个 token 验证贡献，因此 cluster correlation 仍可能是结果的伴随现象。

## Limitations
这是短篇 CIKM 解释分析，任务集中在 neural IR ranking；embedding 几何受模型、层、tokenization 和数据分布影响。公开正文与摘要没有建立跨模型的统一因果定理，不能把距离近直接解释成语义或因果证据。

## 两句话总结
论文把神经 ranker 的解释从 token importance 转向 query/document embedding 的层间几何和聚类变化。它提供了诊断误排序、early exit 和 QPP 的新视角，但主要证据是表示与分数的关联，不是 intervention-level faithfulness。

## Evidence note
已核对公开 CIKM PDF/摘要、方法主张和 ranking explanation 结论；对未公开的额外模型细节没有自行补写。
