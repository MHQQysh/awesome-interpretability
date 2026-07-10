# 115. XRec: Large Language Models for Explainable Recommendation

> 人工精读笔记：Findings of EMNLP 2024。重点是把 user-item graph collaborative signal 注入 LLM，再同时评价推荐准确率和解释质量。

## 0. 论文信息

- **作者**：Qiyao Ma, Xubin Ren, Chao Huang
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.22)
- **代码**：[XRec](https://github.com/HKUDS/XRec)

## 1. Introduction：要解决什么问题

传统 explainable recommender 常依赖 ID embeddings，能排序却难以生成有意义的理由；LLM 能生成自然语言 explanation，但如果没有 user-item collaborative structure，可能忽略高阶邻居和真实偏好。XRec 要在保留图协同信号的同时，让 LLM 生成个性化、可读且与推荐结果关联的 explanation。

## 2. Method / Framework：怎么解决

XRec 用 Collaborative Relation Tokenizer 将用户、物品及其高阶 graph neighborhood 转成适合 LLM 的 token/embedding，并将 graph collaborative filtering 表征替换到 transformed token embeddings。LLM 根据用户历史、候选 item 和 graph context 生成推荐与解释文本。

## 3. Baseline / 对比基线

传统 graph collaborative filtering/recommender；PEPLER 等 sentence-level explainable recommendation；只用 language model 的生成式推荐；无 graph tokenizer 或去掉 collaborative relation 的 XRec 变体。

## 4. Experiments / Findings：结果如何读

在三个 recommendation dataset 上，XRec 在 recommendation metrics 与 explanation metrics 上总体优于 baseline，尤其能利用高阶 user-item dependencies。生成解释不仅复述 item 描述，还能将用户历史偏好与推荐 item 的关联说清楚。

data sparsity 实验显示，交互减少时 XRec 仍保持竞争力，graph signal 能补充稀疏用户历史；但 cold-start 完全没有邻居信息时优势会缩小。

## 5. Ablation / 机制解释

去掉 graph collaborative information、去掉高阶 aggregation、改变 relation tokenizer 和只保留 language prompt，都会削弱推荐或解释。ablation 说明 XRec 的收益来自 graph-LLM 融合，而不是单纯增大生成模型。

## 6. Limitations / 局限与复现注意

自然语言 explanation 的 automatic quality 不能完全证明它忠实于推荐分数；图谱噪声、用户隐私和 cold-start 仍是现实问题；不同数据集的 explanation annotation 和评价指标不完全一致。

## 7. 两句话总结

XRec 将 user-item graph 的高阶协同关系编码为 LLM token，使模型能同时做推荐和自然语言解释。实验显示 graph signal 改善准确率、解释质量和稀疏数据鲁棒性，但解释是否真正对应内部排序依据仍需 causal/用户研究验证。
