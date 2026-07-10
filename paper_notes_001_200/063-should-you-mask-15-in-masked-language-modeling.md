# 063. Should You Mask 15% in Masked Language Modeling?

- **作者 / venue**：EACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.eacl-main.217/)

## Introduction / Method

BERT 传统上 mask 15% token，但这一比例更多是经验惯例。论文系统比较 uniform masking、不同 masking rate 和策略，观察 pretraining loss、下游 GLUE/SQuAD 表现与训练速度，进而讨论表示学习中的信息瓶颈。

## Baseline 与 Findings

以 15% uniform masking 为主要 baseline，与高比例 masking、策略化 masking 和更大学习率/模型设置比较。结果显示，某些模型即使 mask 到 80%，仍保留超过 95% 的 fine-tuning performance；40% 模型在 QNLI/QQP 上可达到相近性能并减少训练时间。

优化每种策略的 masking rate 后，uniform masking 往往与复杂策略相当甚至更好，说明策略复杂度不自动带来更好表示。

## 局限

最佳比例依赖模型规模、数据和下游任务；高 masking 可能损害 token-level reconstruction，不能直接外推到所有预训练目标。

**一句话评价**：论文用系统 ablation 质疑 15% 的惯例，并说明 masking rate 会改变模型学到的表示与训练效率。
