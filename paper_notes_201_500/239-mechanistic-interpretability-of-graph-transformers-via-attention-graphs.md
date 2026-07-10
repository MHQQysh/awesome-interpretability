# 239. Towards Mechanistic Interpretability of Graph Transformers via Attention Graphs

- **Authors:** Batu El, Deepro Choudhury, Pietro Liò, Chaitanya K. Joshi
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2502.12352
- **Tags:** Graph Transformer, Attention Graph, Mechanistic Interpretability, Message Passing

## Introduction

GNN/Graph Transformer 的预测依赖节点间消息传递，但 attention heatmap 很难说明信息怎样跨层传播、是否保留原图结构，尤其在 homophilous 与 heterophilous graph 上。论文利用 message passing 与 self-attention 的数学等价性，构造 Attention Graph 作为 graph model 的机制解释对象。

## Method / Framework

跨层、跨 head 聚合 attention matrices，得到描述节点信息流的 Attention Graph，并 threshold 成 quasi-adjacency matrix 与原始 graph adjacency 比较。作者进一步分析 sparse/dense、biased/unbiased attention、多头与不同深度下的信息传播和节点分类行为。

## Baselines / Comparisons

在 Cora、Citeseer、Chameleon、Squirrel、Cornell、Texas、Wisconsin 上比较 SC/SL/DLB/DL 等不同 attention parameterization 和 1L1H、多层多头模型。指标包括节点分类 accuracy、quasi-adjacency 与原图 F1、层深衰减和 attention noise。

## Experiments / Findings

- dense learned (DL) 模型的 Attention Graph 与原图 F1 低于 4%，说明高分类分数不代表 attention 复现输入 connectivity；带 bias 的 DLB 达到约 28%--86%，更能部分恢复图结构。
- 多头平均通常提高结构 F1，但随着深度增加 F1 下降，说明深层信息流超出原始邻接结构、形成更复杂的 graph computation。
- 在 heterophilous 数据上，attention graph 能帮助区分“模型依赖邻域”与“模型形成的长程/偏置连接”，为解释 graph Transformer 的 message passing 提供可视化和准因果假设。

## Ablation / Error Analysis

消融 head 数、层深、attention bias、threshold 和 graph homophily。threshold 过低保留噪声，过高漏掉弱但重要边；quasi-adjacency F1 低不一定意味着模型错误，因为模型可能学习了超出原图的有效长程关系。

## Limitations

attention aggregation 仍是结构线索，不能单独证明某条 edge causally drives logit；有限 graph benchmark 和合成 attention parameterization 不能代表大型 graph Transformer。对节点特征、非局部 skip connection 和训练动态的解释还需 intervention。

## 两句话总结

Attention Graph 将 graph Transformer 的跨层 attention 聚合成可分析的信息流，并用 quasi-adjacency 检查模型是否保留或改写输入图结构。结果显示深层模型常不直接复现原图，说明 graph 机制解释需要超越 attention heatmap 的结构与因果分析。
