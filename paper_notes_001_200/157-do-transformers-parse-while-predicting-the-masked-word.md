# 157. Do Transformers Parse while Predicting the Masked Word?

- **Authors:** Haoyu Zhao, Abhishek Panigrahi, Rong Ge, Sanjeev Arora
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.1029
- **Tags:** Mechanistic Interpretability, Parsing, Masked LM, Theory

## Introduction

预训练模型的 embedding 可以线性 probe 出 parse tree，但高 probe accuracy 可能只来自与 syntax 相关的词频或浅层 computation，并不意味着模型真的在预测 masked word 时执行 parsing。论文在较现实的 embedding dimension、attention head 和 transformer setting 下研究：transformer 是否能在 masked-word prediction 中隐式构造 constituency structure，以及这种结构为何出现。

## Method / Framework

论文从 BERT-style transformer 和 PCFG 出发，构造理论 / 实验模型，追踪 masked token prediction 所需的上下文信息和层间 attention。它分析 hidden state 中是否存在句法边界、constituent composition 和 token-to-span 信息，并用 controlled synthetic grammar 与真实语言模型作对照。

核心不是只用 probe，而是把预测任务、表示结构和可构造的 parsing mechanism 对齐：如果模型的预测需要跨 span 聚合，且表示中出现可分离的 constituent signal，才更支持“parse while predicting”。

## Baselines / Comparisons

比较没有句法结构的浅层 predictor、不同 embedding dimension / head 数、只用局部上下文的 attention，以及 BERT 等 pretrained masked LM 的 probe 结果。评价包括 masked-word prediction、constituency parsing accuracy、结构可读性和不同层 / head 的贡献。

## Experiments / Findings

- 构造的 transformer 在合理维度和头数下可以同时做 masked-word prediction 与近似 parsing，说明出现句法结构并不需要不现实的无限维度假设。
- hidden state 中的 constituent 信息可以被线性分离；部分 attention pattern 对跨 span 关系和边界判断有明显作用。
- 句法结构并非完全独立于语义：模型能利用 semantics 和 syntax 的互补信号，掩码位置、句法深度和上下文长度会改变可读出程度。
- 结果支持一种中间结论：probe 出的 parse signal 不是纯粹的 post-hoc artifact，但不意味着每次 masked prediction 都显式运行传统 parser。

## Ablation / Error Analysis

层数、embedding dimension、attention head 数、上下文窗口、PCFG 复杂度和 mask position 是主要消融。模型容量太小会失去长距离结构，容量太大则可能通过 shortcut 完成预测。probe 在浅层可能读出词面线索，在深层才更接近 constituent composition；synthetic grammar 与自然语言之间仍有差异。

## Limitations

理论和实验主要针对 BERT-style masked LM 与 PCFG，不覆盖 decoder-only generation 或现代大语言模型的所有架构。线性可分离仍只是 representational evidence，不是对实际神经计算路径的完整因果证明。真实语料中的词义、频率和数据污染可能混入结构解释。

## 两句话总结

论文把“能 probe 出 parse tree”推进为“预测 masked word 时是否可能形成句法结构”的机制问题，证明合理规模的 transformer 可以同时支持预测和近似 parsing。结果削弱了“parse signal 只是相关性”的担忧，但仍不能证明模型每个 token 都按显式 parser 运行。
