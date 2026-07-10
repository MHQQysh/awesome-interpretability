# 121. Interpretable Preferences via Multi-Objective Reward Modeling and Mixture-of-Experts

> 人工精读笔记：Findings of EMNLP 2024。重点是把黑盒 scalar reward 拆成 helpfulness、truthfulness、safety、verbosity 等可解释目标。

## 0. 论文信息

- **作者**：Haoxiang Wang, Wei Xiong, Tengyang Xie, Han Zhao 等
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.620)
- **方法**：ArmoRM，多目标 reward model + MoE gating
- **评测**：RewardBench

## 1. Introduction：要解决什么问题

RLHF reward model 通常把复杂人类偏好压成一个 scalar，难以解释为什么偏好某个回答，也容易继承 verbosity bias 和 reward hacking。模型可能得到高 reward，却因为长度/格式等 shortcut 违背真实偏好。

## 2. Method / Framework：怎么解决

ArmoRM 先对多个偏好维度做 regression，得到 helpfulness、correctness、truthfulness、honesty、safety、verbosity 等 reward vector；再由 gating network 根据 prompt 动态选择各目标权重，MoE scalarization 形成最终 reward。作者还对 verbosity 做 penalty，避免冗长被误当质量。

## 3. Baseline / 对比基线

Llama-3 8B Bradley-Terry RM、GPT-4 judge、固定 reward weights、无 verbosity penalty 的多目标 RM、Nemotron-4 340B RM 等。

## 4. Experiments / Findings：结果如何读

在 RewardBench 上，ArmoRM-8B 明显超过 Llama-3 8B BT reward model 和 GPT-4 judge 相关 baseline，并在复杂 reasoning/安全类别保持竞争力。gating 比固定权重更好，说明不同 prompt 确实需要不同偏好目标；简单平均会把 safety/helpfulness 冲突混为一个分数。

## 5. Ablation / 机制解释

固定 weights vs learned gating、加入/去除 verbosity penalty、单目标 vs 多目标是主要消融。gating 对复杂 preference category 的收益最大，verbosity 调整可减少长度偏差。

## 6. Limitations / 局限与复现注意

训练偏好数据主要为英语；RewardBench 覆盖有限，不能代表真实 RLHF 部署；多目标标签本身可能有噪声，gating 仍不是完全可解释的 causal decision。

## 7. 两句话总结

ArmoRM 把 scalar reward 拆成多个可读偏好目标，再用 prompt-conditioned MoE 动态加权，降低黑盒 reward 和 verbosity bias。RewardBench 结果显示它以 8B 规模超过多个强基线，但目标标注和英语数据分布限制了泛化。
