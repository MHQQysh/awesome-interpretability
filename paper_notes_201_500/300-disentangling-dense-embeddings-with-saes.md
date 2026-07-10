# 300. Disentangling Dense Embeddings with Sparse Autoencoders

- **Authors:** Charles O’Neill, Christine Ye, Kartheik G. Iyer, John F. Wu
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2408.00657
- **Tags:** Dense Embeddings, SAE, Semantic Features, Feature Families

## Introduction

大模型 sentence/document embeddings 通常是 dense vector，适合相似度检索却难以回答“哪一类语义造成相似”。SAE 在 transformer activation 上已能分解 feature，但能否对 embedding model 的 dense semantic space 做同样 disentanglement，是本文的核心问题。

## Method / Framework

作者在超过 420,000 篇计算机科学与天文学论文 abstracts 的 dense embeddings 上训练 SAEs，分析 sparse latent 的 semantic activation examples、重构 fidelity 和不同 dictionary capacity。提出 feature families：把相关但抽象层级不同的 features 聚成家族，观察例如主题、方法、天体对象等从粗到细的语义关系，并用 sparse code 做检索/解释。

## Baselines / Comparisons

比较原始 dense embedding、不同 SAE capacity/sparsity、重构后的 embedding 和 feature-level semantic search；评价包括 reconstruction/semantic fidelity、feature interpretability、稀疏度、feature family coherence 和下游 retrieval behavior。baseline 是 embedding 本身的相似度解释能力，而不是生成模型 circuit。

## Experiments / Findings

SAE sparse representations 保留了 dense embedding 的语义相似性，同时把 activation 分成更易命名的主题 feature；feature families 能表示同一概念的不同抽象层级，便于人理解“为什么两篇摘要相似”。增加 capacity 可发现更细粒度 feature，但需要在 dead/rare feature、稀疏度和解释质量之间取平衡。

## Ablation / Error Analysis

SAE capacity、sparsity、embedding model、领域（CS/astronomy）和 feature family clustering 是主要消融。embedding 中的 feature 更像语义相关方向，不一定是生成模型的内部计算变量；高 cosine similarity 仍可能来自词汇/主题共现，feature name 也可能由 top examples 过度概括。

## Limitations

数据集中于 scientific abstracts，领域分布和语言风格限制泛化；feature family 定义和解释质量部分依赖人工/自动命名。SAE 对最终 embedding 的可解释性不能直接推出 LLM token-level circuit 或 causal faithfulness。

## 两句话总结

论文把 SAE 应用到 42 万篇科学摘要的 dense embeddings，证明 sparse latent 能在保持语义 fidelity 的同时提供可读概念和 feature families。它拓展了 SAE 的对象范围，但 embedding-level semantic feature 与生成模型内部机制仍需要分开评价。

## Evidence note

本笔记逐段核对 arXiv PDF 的 dense-embedding SAE setup、dataset、feature-family analysis、capacity/sparsity experiments 与 conclusion。

