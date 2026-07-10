# 227. Monosemanticity in Recommender Systems

- **Authors:** Yagel Alfasi, Eden Rzezak, Eadan Schechter
- **Venue:** arXiv 2026
- **Paper:** https://arxiv.org/abs/2606.29341
- **Tags:** Recommender Systems, Matryoshka SAE, Monosemanticity, Intervention

## Introduction

矩阵分解推荐器的 latent dimensions 通常没有明确语义，难以审计或精确干预；普通 SAE 在 dictionary 变大时还会 feature splitting、absorption 和 composition。论文把 mechanistic interpretability 的稀疏分解带到 collaborative filtering，研究推荐 embedding 是否含有层级、可解释且可控制的 latent factors。

## Method / Framework

作者在 Amazon Fashion 上训练大规模 matrix-factorization recommender，再对 item/user latent embeddings 训练 Matryoshka Sparse Autoencoder (MSAE)，让不同 dictionary prefix 形成层级稀疏表示。通过 metadata alignment 与 LLM labeling 检查 feature coherence/disentanglement，并对 gender-associated latent neurons 做 post-hoc intervention，观察推荐行为是否可控改变。

## Baselines / Comparisons

与 matching hidden dimension 的 plain SAE 对比，评估 reconstruction 后的 AUC、Precision@10、Recall@10、NDCG@10 以及 monosemanticity/coherence。推荐分数固定 user embeddings，指标反映 item embedding reconstruction 是否保持原 matrix-factorization 排序，而不是重新训练一个推荐器。

## Experiments / Findings

- MSAE 的 AUC 0.7754 高于 SAE 0.7685，Precision@10/Recall@10/NDCG@10 为 0.0012/0.0059/0.0033，略高于 SAE 的 0.0011/0.0057/0.0032；绝对 ranking 值很低，论文将其归因于 Amazon Fashion 极稀疏数据。
- 层级特征能暴露 gender-oriented fashion 等可读概念，LLM labeling 与 metadata alignment 支持 feature coherence；不同 dictionary prefix 提供从粗到细的解释粒度。
- 干预 gender-associated neurons 可以在不重训基础 MF 的情况下改变推荐行为，说明 post-hoc monosemantic feature 不只是描述工具，也可作为可控接口。

## Ablation / Error Analysis

消融 plain SAE vs MSAE、dictionary prefix/size、metadata vs LLM labels 和 intervention strength。feature splitting/absorption 会在大 dictionary 中破坏可读性；性别概念可能与商品类别、品牌或数据采样纠缠，干预也可能造成无关偏好漂移。

## Limitations

单一 Amazon Fashion、极稀疏交互和 latent MF 不能代表神经推荐器、序列推荐或多模态平台；指标接近零时微小绝对提升需谨慎解释。LLM 标签是语义 proxy，gender intervention 涉及公平与刻板偏差，尚需用户效用和安全评测。

## 两句话总结

论文用 Matryoshka SAE 把推荐 embedding 分解成层级 monosemantic features，并展示 gender-oriented latent 可以被解释和干预。MSAE 略好于 plain SAE 且保留排序，但数据稀疏、语义纠缠和公平风险限制了结论。
