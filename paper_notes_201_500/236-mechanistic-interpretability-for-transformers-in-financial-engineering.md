# 236. Mechanistic Interpretability for Transformers in Financial Engineering

- **Authors:** Paper metadata lists a TechRxiv 2025 contribution
- **Venue:** TechRxiv 2025
- **Paper:** https://doi.org/10.36227/techrxiv.173713226.66476652/v1
- **Tags:** Mechanistic Interpretability, Transformers, Finance, Survey/Application

## Introduction

金融工程中的 Transformer 可能影响交易、风险和预测决策，但黑盒输出难以审计；普通 feature importance 也难解释时间依赖、市场 regime 和多变量交互。本文把 mechanistic interpretability 方法带入金融工程，讨论如何定位内部表示并支持更可审计的模型使用。

## Method / Framework

文章围绕 attention/activation analysis、feature attribution、probing、activation patching/steering 和 causal intervention 组织金融 Transformer 的解释流程：先识别时间/因子表征，再检查它们对预测的作用，最后用干预验证。重点是把市场变量、时间窗口和模型内部状态对应起来，而不是只报告相关系数。

## Baselines / Comparisons

比较传统 input-level explainability、attention visualization、feature importance 与 mechanistic/causal tools，评价透明度、稳定性、计算成本和对 distribution shift 的适应性。金融任务中还需要对照不同 market regimes、资产/时间切分和风险指标，避免随机切分产生泄漏。

## Experiments / Findings

- 机制工具能帮助分析 Transformer 是否依赖价格、成交量、技术因子或时间上下文，并为模型错误和 regime shift 提供比单一 importance 更细的假设。
- activation intervention 可用于测试某一 factor representation 是否真的影响预测；但金融数据非平稳，某一时期的 circuit 不一定跨市场保持。
- 论文的主要贡献是把金融风险、合规和可解释需求与 mechanistic workflow 接起来，而不是宣称通用金融 circuit 已被发现。

## Ablation / Error Analysis

需要消融时间窗口、market regime、factor group、attention aggregation 和 intervention strength；否则容易把共同市场趋势当作因果机制。错误还来自数据泄漏、幸存者偏差、低信噪比和模型对历史异常的过拟合。

## Limitations

TechRxiv 版本是非同行评审技术报告，具体金融数据、指标和可复现实验应以原文/代码进一步核对。机制解释不能替代 backtest、风险控制或合规审查，跨资产和未来 regime 的稳定性仍未知。

## 两句话总结

本文把 mechanistic interpretability 的定位、干预和因果验证思路引入金融 Transformer，强调从 input importance 走向可审计内部因素。金融非平稳、数据泄漏和非同行评审状态意味着结论必须谨慎，不能直接当作交易策略证据。
