# 464. Dynamic Brain Transformer with Multi-Level Attention for Functional Brain Network Analysis

- **Authors:** Xuan Kan, Antonio Aodong Chen Gu, Hejie Cui, Ying Guo, Carl Yang
- **Venue / year:** IEEE BHI 2023
- **Paper:** [arXiv:2309.01941](https://arxiv.org/abs/2309.01941)
- **Tags:** `attention-interpretability` `brain-network` `temporal-analysis` `graph-transformer`

## Introduction
静态 functional connectivity 概括整个 fMRI 扫描，却丢失脑功能随时间切换的信息；只用 dynamic FC 又维度高、噪声大。DART 的问题是如何把稳定的全局静态网络和局部动态网络结合，同时通过 attention 指出哪些脑边和时间窗口影响预测。

## Method / Framework
先对 ROI 的完整 BOLD 序列求 Pearson static FC，再用滑窗得到多个 dynamic FC。每个 dynamic graph 经 graph transformer 和 learnable edge attention `alpha` 编码，再加入时间 positional embedding；static embedding 作为 anchor，与各 dynamic embedding 算相似度得到 temporal attention `beta`，加权融合后做分类/回归。

## Baselines / Comparisons
动态 baseline 为 STAGIN、ST-GCN；静态 baseline 为 BrainGNN、BrainGB、BrainNetCNN、BNT。数据为 PNC 分类和 ABCD cognition-summary regression，指标分别为 AUROC/accuracy 与 MSE，结果为五个随机 seed 的均值。

## Experiments / Findings
PNC AUROC/accuracy 和 ABCD MSE：STAGIN 63.5/54.2/102.4，ST-GCN 64.7/57.3/89.2，BNT 78.2/70.6/60.2，DART 80.7/72.5/58.3。静态网络提供最强 global signal，动态网络在 static anchor 引导下补充细粒度状态变化。

## Ablation / Error Analysis
论文主要用 baseline family 对照，没有完整的去掉 alpha/beta 的数值 ablation。attention 可视化显示 edge-level attention 从均匀逐渐集中到 Visual/Default Mode subnetworks，temporal attention 偏向中间采集窗口；这给出神经科学可解释假设，但仍是注意力权重分析而非严格因果干预。

## Limitations
研究不属于 LLM 主线，且 fMRI 数据、ROI atlas、滑窗参数和低 SNR 都限制泛化。attention 的高权重不能自动等于脑区的因果重要性，跨队列/任务验证仍需要独立实验。

## 两句话总结
DART 用 static FC 作为 query/anchor，把 dynamic FC 的空间边和时间片同时纳入 Transformer attention。它在 PNC/ABCD 上超过所列静态和动态 baseline，并提供可读的 edge/time hypotheses，但 attention 解释仍需神经科学验证。

## Evidence note
已读取本地 arXiv 正文的 FC 构造、alpha/beta 两级 attention、数据设置、表 1 和 attention visualization；该条目是脑网络邻近工作，已明确标注其不属于 LLM 主线。
