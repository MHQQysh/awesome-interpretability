# 408. Does Circuit Analysis Interpretability Scale? Evidence from Multiple Choice Capabilities in Chinchilla

- **Authors:** Tom Lieberum, Matthew Rahtz, János Krámár, Neel Nanda, Geoffrey Irving, Rohin Shah, Vladimir Mikulik
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2307.09458
- **Tags:** `mechanistic-interpretability` `circuit-analysis` `scaling` `Chinchilla`

## Introduction
Circuit analysis 在小 Transformer 上已经解释过 induction、IOI 等行为，但研究者担心模型规模变大后，组件数量、冗余和分布式表示让解释不可扩展。本文用 Chinchilla 系列的 multiple-choice capabilities 测试 circuit analysis 是否能随规模推进。

## Method / Framework
作者围绕 multiple-choice prediction 做 activation/weight attribution、component ablation 和 circuit tracing，识别 attention heads、MLP features、token positions 对答案 logit 的贡献，再比较不同参数规模和任务。重点不是生成一张漂亮图，而是测量解释覆盖率、faithfulness 与分析成本是否随规模恶化。

## Baselines / Comparisons
比较完整 Chinchilla 模型、不同规模 checkpoint、随机/均匀 component、gradient/activation attribution 和人工或自动选择的 circuit。行为 baseline 是模型在 multiple-choice 任务上的答案准确率；机制 baseline 是只保留候选 circuit 后能否恢复原输出。

## Experiments / Findings
结果显示某些局部 circuit 分析可以在中等规模模型上解释 multiple-choice 行为，但规模增加并不自动带来同等清晰度；能力更多由分布式、冗余和上下文依赖共同产生。即使找到高 attribution 组件，也需要 ablation 才能检查它们是否真正必要。

## Ablation / Error Analysis
作者比较不同 attribution method、component subset 和 circuit threshold。删太少会保留大量冗余，删太多会破坏答案；只看单层或单个 head 会漏掉跨层组合。模型可能利用多个可替代路径，因此“一个 circuit”常是足以完成任务的子图，不一定是唯一机制。

## Limitations
multiple-choice 任务比开放式语言行为更容易做 circuit tracing，不能代表所有 LLM 能力。解释成本、人工先验、数据集和 checkpoint 选择会影响结论；规模实验也不等于证明 circuit analysis 在万亿参数模型上经济可行。

## 两句话总结
本文检验 circuit analysis 是否能从小模型扩展到 Chinchilla 的 multiple-choice 能力，发现分布式和冗余路径使规模化解释更困难，不能只看 attribution。它强调 faithfulness、ablation 和稀疏性必须一起报告，单个高分 head 不足以构成可扩展机制解释。

## Evidence note
已读取本地 PDF 的 Chinchilla scaling、multiple-choice circuit、attribution/ablation 和局限章节。
