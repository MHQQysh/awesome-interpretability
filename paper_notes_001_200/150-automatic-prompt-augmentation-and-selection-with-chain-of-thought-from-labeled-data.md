# 150. Automatic Prompt Augmentation and Selection with Chain-of-Thought from Labeled Data

- **Authors:** Kashun Shum, Shizhe Diao, Tong Zhang
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.811
- **Tags:** Prompt Optimization, Chain-of-Thought, Policy Gradient, Few-shot

## Introduction

CoT 依赖人工设计的 rationale exemplars，但 order、complexity、diversity 和 writing style 都会改变结果；有 labels 却没有 rationale 的真实数据集无法直接使用 Manual-CoT。作者提出 Automate-CoT，让模型从少量 labeled questions 自动扩增、筛选和组合 rationale chains。

## Method / Framework

1. **Augment。** 对训练问题采样多个 pseudo-CoT。
2. **Prune。** 只保留最终答案与 ground truth 一致的 chain，利用“答案正确是 rationale 正确必要条件”的近似降低噪声。
3. **Select。** 把候选 chains 作为策略动作，用 variance-reduced policy gradient 估计每个 exemplar 的贡献，选择 4 到 8 个兼顾复杂度和多样性的组合。

选出的组合用于新问题的 few-shot CoT，不需要人工逐题编写 rationale。作者还比较 self-consistency，并分析训练池规模、顺序、hop 数、风格和随机性。

## Baselines / Comparisons

比较 Manual-CoT、Auto-CoT、random exemplar selection、self-consistency、Complex-CoT、PromptPG 和 retrieval-based prompt selection。任务覆盖 GSM8K、ASDiv、SVAMP、AQuA、SingleOp、CommonsenseQA、StrategyQA、Last Letter、OpenBookQA、e-SNLI、SST-2；指标为 exact match accuracy。

## Experiments / Findings

- Automate-CoT 相对 Manual-CoT 在 arithmetic、commonsense、symbolic 和 non-reasoning 任务分别提升约 2.7%、3.4%、3.2% 和 2.5%。
- 相对 Auto-CoT，GSM8K 提升约 4.8%；在多个数据集上自动选择的候选比随机选择稳定，平均提升约 3.3%。
- 自动生成的复杂 chains 能覆盖人工 CoT 较少覆盖的 4 到 8 hop 问题；在 GSM8K 复杂问题上，Complex-CoT 比只用简单 3-hop chain 更有效。
- 方法对训练集随机抽样较鲁棒；选出的 exemplars 能比未选 exemplars 高约 2%，说明 policy-gradient selection 学到的是任务相关性，而非单纯链长度。
- 在 gpt-3.5-turbo 上也保持总体改进，但不同模型的最佳 exemplar 数和收益不同。

## Ablation / Error Analysis

作者分别消融 order、complexity、diversity、style、候选池大小、selection policy 和 self-consistency。随机打乱 Manual-CoT 会造成明显波动；池子过小无法覆盖复杂推理，池子继续扩大则成本快速增加。只按答案正确 prune 仍会保留“答案碰巧正确但中间推理错误”的 rationale；不同 style 的 chains 对模型敏感，说明 prompt selection 并不是纯组合优化。

## Limitations

伪 rationale 的正确性依赖 ground-truth answer，不能保证每一步推理忠实；生成和评估大量候选需要多次 API 调用。policy gradient 的验证集选择可能过拟合，跨任务迁移也不总是稳定。方法仍依赖有 labels 的训练数据，不能替代完全无监督 self-improvement。

## 两句话总结

Automate-CoT 将 CoT prompt engineering 拆成生成、答案一致性过滤和策略梯度选择，自动找到适合任务的多样 reasoning exemplars。它在多类推理任务上超过 Manual-CoT / Auto-CoT，但“答案对”不等于“解释对”，候选生成成本和 rationale 噪声仍是主要限制。
