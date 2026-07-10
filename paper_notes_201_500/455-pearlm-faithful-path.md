# 455. Faithful Path Language Modeling for Explainable Recommendation over Knowledge Graph

- **Authors:** Giacomo Balloccu, Ludovico Boratto, Christian Cancedda, Gianni Fenu, Mirko Marras
- **Venue / year:** RecSys 2024
- **Paper:** [arXiv:2310.16452](https://arxiv.org/abs/2310.16452)
- **Tags:** `explainable-recommendation` `knowledge-graph` `faithfulness` `constrained-decoding`

## Introduction
路径推理推荐器把 user-item 关系写成自然语言路径，但 PLM 可能生成知识图谱中不存在的 triplet，导致解释看似详细却不真实。论文先量化这种 hallucination 对用户信任的损害，再提出 PEARLM，使推荐路径和 KG 结构同时受到约束。

## Method / Framework
PEARLM 用 causal language modeling 学习 user-centric KG paths，直接从 KG 路径学习 entity/relation embedding，而不是依赖独立训练的 TransE 初始化。核心是 KG-constrained decoding (KGCD)：每一步只允许当前实体在图中可达的 relation/entity token，因此生成路径的 triplet 都能在 KG 中验证。作者定义 PFR@k：截至第 k hop 未含 corrupted triplet 的路径比例。

## Baselines / Comparisons
推荐性能比较 CKE、CFKG、KGAT、PGPR、UCPR、CAFE、原始 PLM 和 PEARLM，数据为 MovieLens-1M 与 LastFM-1M，按用户时间 60/20/20 划分。解释感知分析还比较 baseline explanation、collaborative explanation、人工准确 path reasoning 和 PLM 产生的 corrupted path；用户研究含 blind/informed 两组。

## Experiments / Findings
原始 PLM 的 PFR 随 hop 下降：MovieLens 为 0.58/0.35/0.06，LastFM 为 0.65/0.24/0.10。PEARLM 在 ML1M 的 NDCG 为 0.44、LFM1M 为 0.59，分别超过第二名约 42% 和 78.7%；ML1M/LFM1M 的 MRR 为 0.31/0.51。KGCD 把 PFR@3 提升到 1，说明路径可验证性不是事后打分，而是解码时保证。

用户研究有 164 名 Prolific participant：准确 path reasoning 比 baseline/collaborative explanation 的 transparency/trust 更好，错误 path reasoning 会降低满意度和信任；总体只有 23.3% 的参与者成功识别真实不准确路径，说明人类不能可靠充当后处理过滤器。

## Ablation / Error Analysis
直接学习 embedding 是主要收益来源：PLM direct-learning 的 ML1M/LFM1M NDCG 0.40/0.51，PEARLM direct-learning 0.44/0.59；TransE 初始化显著较差。去掉 KGCD，PEARLM NDCG 略升到 0.45/0.60，但 PFR@3 降为 0.11/0.14；5-hop 路径比 3-hop 更噪，模型变大也没有稳定改善，显示推荐 utility 与解释 faithfulness 的 trade-off。

## Limitations
KGCD 在用户可达路径少于 top-k 或路径更长时会限制候选空间；实验只覆盖两个数据集，知识图谱缺失事实也会被“忠实”地保留。更高 NDCG 不等于解释因果真实，路径仍只解释模型依赖的 KG 连接。

## 两句话总结
PEARLM 把 KG 约束放进语言模型解码，解决了 path-based recommender 生成不存在关系的问题，并用 PFR@k 量化路径忠实性。直接学习 embedding 提升推荐，KGCD 保证结构正确，但长路径和稀疏图上的候选覆盖仍需权衡。

## Evidence note
已逐段核对探索性用户研究、PFR 定义、PEARLM/KGCD、主表以及 embedding/KGCD/path-length ablation。
