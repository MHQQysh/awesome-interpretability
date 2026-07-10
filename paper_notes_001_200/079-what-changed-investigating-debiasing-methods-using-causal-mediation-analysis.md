# 079. What changed? Investigating Debiasing Methods using Causal Mediation Analysis

- **作者 / venue**：GEBNLP 2022
- **论文**：[ACL Anthology](https://aclanthology.org/2022.gebnlp-1.26/)

## Introduction / Method

debiasing 方法通常只报告 bias metric 和 downstream accuracy，却不解释模型内部发生了什么。论文用 causal mediation analysis 分解 neuron、layer、attention head 对 bias 的 mediation effect，比较 debias 前后哪些路径被改变。

## Baseline 与对比

比较多种 debiasing method、不同 bias metric 和 downstream task。重点不是一个方法赢了多少，而是同一方法在不同 metric 下是否一致、哪些层/模块承载 bias。

## Findings

debiasing 可能降低某些 bias score，却把影响转移到其他层或指标；downstream task 也可能受损。因果 mediation 显示部分 gender association 主要通过特定层/路径传播，提供比输出分数更细的解释。

## 局限

mediation 结果依赖 mediator 选择和干预模型；社会 bias metric 本身也不完整。不能把一个被改变的 pathway 直接称为全部 bias 机制。

**一句话评价**：论文把 debiasing 从“分数变化”推进到“哪条内部因果路径发生了变化”。
