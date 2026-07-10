# 318. How Large Language Models Encode Context Knowledge? A Layer-Wise Probing Study

- **Authors:** Tianjie Ju, Weiwei Sun, Wei Du, Xinwei Yuan, Zhaochun Ren, Gongshen Liu
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2402.16061
- **Tags:** Layer-wise Probing, Context Knowledge, V-Usable Information, Conflict Resolution

## Introduction

LLM 能从 prompt 中读取事实，但 context knowledge 在哪一层编码、何时与 parametric knowledge 融合、冲突时如何选择仍不清楚。论文构造多样、连贯的 evidence/fact probing dataset，做 layer-wise study。

## Method / Framework

作者用 ChatGPT 生成与事实对应的 context evidence，设计 context retrieval、knowledge encoding、conflict/consistency 等 probing tasks；对不同层 hidden representation 训练 probes，并用 V-usable information 衡量 representation 对 context label 的可用信息，而不只看 probe accuracy。

## Baselines / Comparisons

比较不同模型层、context/no-context、consistent/conflicting evidence、不同 probe/classifier 和 parameter knowledge condition。V-usable information 与随机/浅层 baseline 对照，旨在分离模型读取 context 的能力和 probe 自身容量。

## Experiments / Findings

层级分析显示 context information 不是一开始就完全存在：中后层逐步编码并与 answer prediction 对齐；当 context 与 parametric knowledge 冲突时，不同层提供不同证据，最终输出可能依赖模型先验与 prompt salience。V-usable information 能更细地显示“表示中有信息但未被简单 probe 读出”的情况。

## Ablation / Error Analysis

evidence quality、context length、conflict direction、layer、probe class 和 dataset generation 是关键消融；ChatGPT 合成 evidence 可能有模板/污染，probe 可能学到 lexical cues。需要 human-verified facts、randomized wording 和 causal patching 才能证明 context 真被模型使用。

## Limitations

probing 是相关性证据，不代表 layer 内部 causal storage；合成 context 和有限模型限制泛化。V-usable information 仍依赖 probe family/estimator，不能完全消除测量偏差。

## 两句话总结

该研究用 V-usable information 追踪 LLM 各层如何编码和使用 prompt context knowledge，并分析参数记忆冲突。它把“模型知道 context”拆成层级过程，但 probe 与合成 evidence 仍需要干预和真实数据验证。

## Evidence note

本笔记逐段核对 arXiv PDF 的 dataset construction、layer-wise probing、V-usable information、conflict experiments 与 limitations。

