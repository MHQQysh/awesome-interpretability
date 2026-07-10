# 226. How Do LLMs Encode Scientific Quality? An Empirical Study Using Monosemantic Features from Sparse Autoencoders

- **Authors:** Michael McCoubrey, Angelo Salatino, Francesco Osborne, Enrico Motta
- **Venue:** arXiv 2026
- **Paper:** https://arxiv.org/abs/2602.19115
- **Tags:** Sparse Autoencoder, Scientific Quality, Monosemantic Features, Scientometrics

## Introduction

LLM 已被用于评审、总结和生成科学论文，但它判断“研究质量”的内部依据并不透明；引用数、期刊 SJR 和 h-index 又可能被领域、文体和发表类型混淆。论文研究 LLM activation 中是否有可解释 monosemantic features 表示 scientific quality，而不是只用一个黑盒预测分数。

## Method / Framework

作者从 LLM activations 训练 sparse autoencoders，筛出与科学质量相关的稀疏 features，并通过 feature activation、文本触发模式和自动/人工标签解释其语义。随后把这些 feature 作为 predictors，分别预测 citation count、journal SJR 和 journal h-index，在不同实验设置下比较其跨任务/跨数据稳定性。

## Baselines / Comparisons

对比原始 dense activation/传统 attribution 与 SAE monosemantic feature predictors，按三类 scientometric target 评价。分析维度包括 IID/OOD 或不同数据切分、feature sparsity、预测相关性和可解释性，而不是把 citation、SJR、h-index 当作同一个质量标签。

## Experiments / Findings

- SAE features 捕捉到多维质量线索，反复出现四类语义：研究方法、出版物类型（综述往往更高影响）、高影响研究领域/技术，以及特定科学术语。
- 同一组稀疏 feature 能在 citation、SJR、h-index 任务中作为预测信号，说明 LLM 表示的不只是词频，而包含论文结构和学术语域模式；但不同指标对“quality”的定义不完全一致。
- feature-level 解释把“模型认为高质量”拆成可审计的术语/方法/类型线索，为分析 scientific evaluation bias 提供入口。

## Ablation / Error Analysis

消融 SAE dictionary、feature selection、任务目标和数据切分。稀疏性过强会漏掉分布式质量信息，过弱又恢复 polysemantic features；术语可能只是领域/期刊 shortcut，综述高影响也可能反映引用习惯而非内在科学价值。

## Limitations

引用数和期刊指标是有偏的 proxy，不能等同于科学质量；自动 feature label 也不能保证模型在推理时因果使用它。研究领域、语言、时间窗口和 LLM 选择有限，跨学科、公平评审和真实专家判断仍需验证。

## 两句话总结

论文用 SAE 把 LLM 的 scientific quality 表示拆成方法、出版类型、领域技术和术语等 monosemantic features，并测试其对三类 scientometric 指标的预测能力。结果说明模型编码了丰富学术模式，但这些模式可能混入领域与发表偏差，不能直接当作客观质量。
