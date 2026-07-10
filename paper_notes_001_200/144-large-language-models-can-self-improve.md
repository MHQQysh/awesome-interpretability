# 144. Large Language Models Can Self-Improve

- **Authors:** Jiaxin Huang, Shixiang Shane Gu, Le Hou, Yuexin Wu, Xuezhi Wang, Hongkun Yu, Jiawei Han
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.67
- **Tags:** Self-Training, Chain-of-Thought, Self-Improvement, Distillation

## Introduction

大模型可以通过 few-shot CoT 在新任务上推理，但进一步提升通常需要人工标注 rationale 和答案。论文问：如果只有未标注的问题，模型能否靠自己的多条推理路径生成训练目标并 self-improve？

## Method / Framework

LMSI（Language Model Self-Improved）先对未标注输入使用 few-shot CoT，采样多条 reasoning paths；用 self-consistency 的 majority voting 过滤高置信答案；将问题、rationale 和答案作为 pseudo-label，微调同一个 PaLM 540B。训练数据来自多个任务，既测试 in-domain，也测试 OOD generalization。

低资源设置中，作者进一步让模型生成额外问题和 CoT prompt，避免连训练问题和人工示例都充足的假设。训练后比较 greedy 和多路径 self-consistency 两种推理。

## Baselines / Comparisons

比较原始 PaLM few-shot CoT、self-consistency、zero-shot “Let’s think step by step”、以及只在单任务或单一 reasoning path 上 self-train。任务包括 GSM8K、OpenBookQA、ANLI-A3 和多个 OOD 任务，重点不是与更大模型比，而是判断同一模型用无标签数据能否超过自己的 prompt baseline。

## Experiments / Findings

- PaLM 540B 在 GSM8K 从 74.4% 提升到 82.1%，OpenBookQA 从 90.0% 到 94.4%，ANLI-A3 从 63.4% 到 67.9%。
- 提升不只发生在训练任务，也扩展到 OOD 任务，说明多任务 pseudo-rationale 能改善更一般的 reasoning pattern。
- 极低资源设置中，LMSI 达到 GSM8K 74.2%，高于 Kojima zero-shot CoT 的 43.0% 和简单扩展的 70.1%。
- self-generated rationale 不只是答案标签：训练目标包含中间推理路径，使模型学习如何组织 reasoning，而非只拟合多数答案。

## Ablation / Error Analysis

多样 reasoning paths 是最关键的因素；只用单条 greedy path 会减少收益，甚至会放大系统性错误。作者还消融训练样本格式、采样 temperature、是否保留 rationale、任务混合比例和训练步数。温度过低降低多样性，过高引入错误 pseudo-label；majority voting 可以过滤一部分错误，但不能保证多数答案正确。

## Limitations

方法依赖模型已有较强的 few-shot CoT 和 self-consistency 能力，小模型不一定能产生足够可靠的 pseudo-label。未标注数据仍可能含有分布偏差和污染，错误会自我训练并循环放大；论文的 PaLM 540B 计算成本也很高。self-improvement 的提升可能来自训练分布和重复曝光，不能完全解释为“模型学会了新推理算法”。

## 两句话总结

LMSI 用多路径 CoT、自一致性投票和 pseudo-rationale 微调，让 PaLM 在没有人工答案的情况下显著提高数学、常识和 NLI 推理。它证明无标签 self-training 可以带来跨任务收益，但前提是 teacher 生成的多样路径足够可靠，且计算与错误累积风险很大。
