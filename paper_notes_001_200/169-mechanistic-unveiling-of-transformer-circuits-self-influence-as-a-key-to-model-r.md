# 169. Mechanistic Unveiling of Transformer Circuits: Self-Influence as a Key to Model Reasoning

- **Authors:** Lin Zhang, Lijie Hu, Di Wang
- **Venue:** Findings of NAACL 2025
- **Paper:** https://aclanthology.org/2025.findings-naacl.76
- **Tags:** Mechanistic Interpretability, Circuit, Self-Influence, IOI

## Introduction

Transformer 的非线性高维交互让 reasoning circuit 难以追踪。已有 circuit analysis 能找子图，但很少沿着每个 token 在不同层的影响变化，解释模型如何从实体识别、动作理解到 pronoun linking 完成多步推理。

## Method / Framework

SICAF（Self-Influence Circuit Analysis Framework）三步：先用自动 circuit-finding 找与任务相关的 subgraph；再计算每个 token 在各层 / head / component 的 self-influence；最后沿层追踪 influence trajectory，映射到人类可读的 reasoning stages。实验以 GPT-2 的 Indirect Object Identification（IOI）为主。

## Baselines / Comparisons

比较全模型 attribution、已有 circuit discovery、token-level influence、attention / activation importance 和不同 circuit selection。指标包括 circuit faithfulness、token ranking stability、IOI accuracy 与人工可解释的 reasoning stage alignment。

## Experiments / Findings

- GPT-2 的关键参数影响主要集中在首层和最后几层，中间层承担连接和路由作用。
- self-influence 随层变化能追踪 token 角色转换：姓名、动作、指代对象在不同阶段成为高影响 token。
- 找到的 circuit 在 IOI 上保留主要行为，且 influence trajectory 比单一最终层 salience 更容易对应到多步 reasoning。
- 多种 circuit identification 方法给出的关键组件部分一致，但精确边界和 influence scale 对方法敏感。

## Ablation / Error Analysis

消融 circuit 大小、层、token self-influence、component selection 和 IOI prompt variation。删掉早期或末层关键组件会显著破坏输出；只看 attention weight 会漏掉 MLP / residual contribution。错误解释常把高 self-influence 当成语义原因，实际上它可能只是输出层的放大效应。

## Limitations

实验集中于 GPT-2 和 IOI，不能代表现代 instruction / reasoning LLM。self-influence 计算仍昂贵，无法轻易扩展到所有参数和长序列；从 influence trajectory 到“模型思维步骤”仍是研究者解释，不是模型显式报告。需要更多任务上的 causal ablation 和 independent replication。

## 两句话总结

SICAF 用 circuit 加 token self-influence 的层间轨迹，把 IOI 的计算路径拆成更接近人类步骤的 reasoning stages。它提供了比单层 salience 更有结构的机制解释，但模型规模、任务范围和 influence 到语义的映射仍限制结论。
