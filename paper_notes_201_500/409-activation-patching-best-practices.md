# 409. Towards Best Practices of Activation Patching in Language Models: Metrics and Methods

- **Authors:** Fred Zhang, Neel Nanda
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2309.16042
- **Tags:** `activation-patching` `causal-intervention` `evaluation` `mechanistic-interpretability`

## Introduction
Activation patching 用 clean/corrupted prompt 对，在某层/位置把 clean activation 替换进 corrupted run，以测量一个组件是否恢复目标行为。它已成为机制解释的主工具，但不同论文使用不同 corruption、metric、normalization 和 patch site，数值难比较，且容易把非因果相关误读成 localization。

## Method / Framework
作者系统梳理 patching 的变量：intervention type、clean/corrupt pair、patch position/layer/head、logit difference/accuracy/KL 等 metric，以及 indirect effect、normalized effect 和 denoising/なく? score。通过 toy Transformer 和语言模型案例比较这些选择对结论的影响。

## Baselines / Comparisons
比较 raw logit difference、probability difference、KL、accuracy-based metric、random patch、mean activation patch、resample ablation 和不同 corruption strength。理想 metric 应在 null/noise 下稳定、对目标行为敏感，并与最终行为因果效果一致。

## Experiments / Findings
不同 metric 可能给出不同“最重要层/位置”；logit difference 常比 accuracy 更有连续分辨率，但受 token choice、基线和 logit scale 影响。corruption 越强不一定越好，可能制造分布外状态；patching 的结果需要做 normalization、重复样本和 clean/corrupt direction 检查。

## Ablation / Error Analysis
主要消融是 metric、patch granularity、corruption distribution 和 normalization。作者建议报告完整/部分 patch、random baseline、多个 seeds 和 confidence intervals，避免只挑一个最漂亮的 heatmap。patch 后行为恢复也可能来自 shortcut 或冗余路径，不证明被 patch 的组件是唯一机制。

## Limitations
activation patching 仍依赖研究者选择的 clean/corrupt pairs 和假设性 metric，计算成本随 layers/positions 迅速上升。它测的是 intervention effect，不自动产生语义标签；复杂生成任务的目标 logit 和 output equivalence 也难定义。

## 两句话总结
本文把 activation patching 的 corruption、patch site、metric 和 normalization 系统化，说明不同实践可能得到不一致的机制结论。它提供的主要价值是实验规范：用因果干预、随机/空白基线、合理归一化和多样样本共同验证，而不是只读一张 attribution 图。

## Evidence note
已读取 arXiv 2309.16042 本地 PDF 的方法、metric 对比、实验建议和 reproducibility/limitations 讨论。
