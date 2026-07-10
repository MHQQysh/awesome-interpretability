# 042. Spectral Filters, Dark Signals, and Attention Sinks

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.263/)
- **任务**：扩展 logit lens，分析 residual stream 中不可直接投影到词表的 spectral subspace

## Introduction

Logit lens 把中间 hidden state 投影到 vocabulary，能显示模型在每层“倾向预测什么”。但 vocabulary embedding/unembedding 只覆盖一部分方向，某些 residual signal 对 logits 不直接显现，却可能被后续 MLP 使用。论文把这些不可见方向称为 dark signals，并研究 attention sinks 与它们的关系。

## Method

- 对 vocabulary embedding/unembedding 矩阵做 SVD。
- 按 singular-vector bands 构造 spectral filters，分别保留/移除不同频段的 representation。
- 将中间表示投影到不同 spectral subspace，形成 logit spectroscopy。
- 做 canonicalization、dimension compression 和 intervention，测试被过滤方向对 NLL、attention sink 和模型行为的影响。

## Baseline 与对比

- 标准 logit lens。
- 随机正交方向 removal，作为“删掉同样维度但不带语义”的 baseline。
- 不同 Pythia size、Llama2 7B/70B 和不同 layer。
- 同时比较 dark-to-embedding、dark-to-unembedding 等 subspace，避免把所有不可见方向混为一谈。

## Findings

- 过滤某些 spectral bottleneck 会显著提高 NLL，超过随机方向删除，说明这些 dark subspaces 不是纯冗余。
- 某些层的 residual stream 可能没有用满全部维度，attention sink 与特定 subspace 有关联。
- MLP 可能把信息写入 logit lens 看不到的方向，随后由其他组件读取；只看词表投影会漏掉这部分机制。
- 论文将 logit lens 从 token prediction visualization 推进到 subspace-level analysis。

## 局限

spectral band 的解释依赖 embedding matrix 和 canonicalization；dark signal 不等于单一语义功能。过滤实验会改变模型分布，必须用随机方向、不同层和不同模型做 controls。

**一句话评价**：论文提醒我们，logit lens 的“看不见”不等于模型没有信息，并提供了频谱干预来分析 residual stream 的隐藏容量。
