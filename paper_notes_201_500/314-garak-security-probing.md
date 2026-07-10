# 314. garak: A Framework for Security Probing Large Language Models

- **Authors:** Leon Derczynski, Erick Galinkin, Jeffrey Martin, Subho Majumdar, Nanna Inie
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2406.11036
- **Tags:** LLM Security, Red Teaming, Probing, Vulnerability Evaluation

## Introduction

LLM 应用不断更新，攻击方式和 failure mode 也快速变化，静态安全 benchmark 难以覆盖 prompt injection、jailbreak、hallucination、data leakage 和 toxicity。garak 的问题是提供可扩展、可复现、自动化的 security probing framework，让研究者在部署前系统发现漏洞。

## Method / Framework

garak 采用 probes（攻击/测试生成器）、detectors（输出判定器）、generators（被测模型接口）和 reporting pipeline 的模块化设计。它可对同一模型运行多类 prompt probes，检测 unsafe、hallucinated、leaked 或 policy-violating output，并保存配置、输入输出和 detector 结果，便于回归测试。

## Baselines / Comparisons

框架不是单一攻击算法，比较维度是不同 probe/detector、模型后端、攻击类别与安全配置；baseline 可包括无 probing、手工 red team、静态 benchmark 和单一 jailbreak suite。评价关注 coverage、reproducibility、false positives/negatives、运行成本和新模型适配。

## Experiments / Findings

garak 将 LLM security 变成可组合的 automated testing workflow，能在模型更新后重复运行并比较 vulnerability profile。它的主要贡献是生态/工程化：把攻击提示、检测规则、模型接口和报告统一起来，而不是声称一次扫描证明模型安全。

## Ablation / Error Analysis

probe selection、detector threshold、模型 temperature、API wrapper 和攻击成功定义都会改变结果；detector 可能漏掉语义变体或误报安全讨论。测试集污染、adaptive attacker 和多轮上下文会让单轮 probes 失效，因此需要持续 red teaming 与人审。

## Limitations

安全 probing 只能发现已覆盖的 failure modes，不能证明不存在漏洞；外部 API、敏感 prompt、成本和法律/隐私边界也限制大规模运行。detector 与被测模型可能共享偏差。

## 两句话总结

garak 用 probes、detectors、model generators 和报告模块把 LLM 安全评估变成可重复的自动化 red-team pipeline。它降低了回归测试门槛，但 coverage、detector 质量和 adaptive attack 使“扫描通过”不能等于安全。

## Evidence note

本笔记逐段核对 arXiv PDF 的 security framing、framework components、probe/detector design、evaluation workflow 与 limitations。

