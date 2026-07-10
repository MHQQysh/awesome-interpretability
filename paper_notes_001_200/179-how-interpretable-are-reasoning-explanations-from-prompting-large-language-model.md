# 179. How Interpretable are Reasoning Explanations from Prompting Large Language Models?

- **Authors:** Wei Jie Yeo, Ranjan Satapathy, Rick Siow Mong Goh, Erik Cambria
- **Venue:** NAACL Findings 2024
- **Paper:** https://aclanthology.org/2024.findings-naacl.138
- **Tags:** CoT, Faithfulness, Robustness, SEA-CoT

## Introduction

CoT 看起来像推理轨迹，但可读、能说服人不等于 faithful。已有工作多只测 faithfulness，忽视 explanation robustness、utility 和不同 prompting technique 之间的差异。

## Method / Framework

作者在多个 commonsense reasoning benchmark 上比较 CoT、Self-Consistency、Least-to-Most 等 prompting，并从 faithfulness、robustness、utility 多维评估。提出 Self-Entailment-Alignment CoT（SEA-CoT）：类似 self-consistency，但要求每一步 reasoning 与支持上下文相互 entail，再选择答案。

## Baselines / Comparisons

比较 zero-shot CoT、few-shot CoT、self-consistency、least-to-most、其它 prompting 和 SEA-CoT。指标包括 answer accuracy、explanation faithfulness、扰动鲁棒性、人类可用性与 step-level entailment。

## Experiments / Findings

- 普通 CoT 的 explanation 常 plausibly correct，却不一定反映模型真正决策过程；answer accuracy 与 explanation faithfulness 不能互换。
- SEA-CoT 在多个 interpretability dimensions 上提升超过 70%，同时保持或改善任务性能。
- 只看最终答案的 self-consistency 会忽略中间冲突；step-level self-entailment 能减少不被 context 支持的跳步。
- explanation utility 与 faithfulness 仍不完全一致，用户可能偏爱流畅但错误的 chain。

## Ablation / Error Analysis

消融 self-entailment、step selection、prompt type、reasoning length 和 benchmark。错误主要来自中间步骤虽与相邻句子相容却不支持最终结论、模型重复答案、以及 entailment judge 本身误判。

## Limitations

SEA-CoT 增加推理和 judge 成本，entailment 不是完整 causal faithfulness。实验集中在 commonsense reasoning 和 prompt-based black-box evaluation，不能直接证明模型内部机制被修正。

## 两句话总结

论文系统区分 CoT 的可读性、faithfulness、robustness 和 utility，显示“看起来合理”远不够。SEA-CoT 用逐步 self-entailment 约束解释，在多个维度显著改善，但仍依赖外部判定器而非内部因果证据。
