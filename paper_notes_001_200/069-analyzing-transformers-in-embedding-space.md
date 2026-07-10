# 069. Analyzing Transformers in Embedding Space

- **作者 / venue**：ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-long.893/)

## Introduction / Method

多数解释方法需要运行模型和输入，但 Transformer 参数本身也可能揭示功能。论文将 FFN value vectors 和相关参数投影到 embedding/vocabulary space，分析它们对应的 token/概念，并用参数相似性解释不同模型/seed 的一致结构。

## Baseline

比较参数直接投影与随机 hidden state 投影；不同模型/seed 的 parameter alignment；以及不运行 forward pass 的静态分析与 activation-based 方法。

## Findings

参数投影能在没有训练/输入的情况下得到有意义的词汇方向；在 IMDB 上从参数空间构造的 classifier 可达到约 **70%** accuracy。某些 layer 的 value vectors 与 vocabulary evidence 对齐，说明 FFN 可能存储可直接读出的 lexical/semantic feature。

## 局限

参数投影的解释依赖 embedding basis 和 top-k；大模型的 zigzag/不稳定现象显示静态解释并不总是简单。静态相关性仍需输入条件 activation 和 intervention 验证。

**一句话评价**：论文展示了“不跑模型也能看参数语义”的路线，但参数可读出不等于参数单独决定行为。
