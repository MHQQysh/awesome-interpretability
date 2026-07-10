# 315. Probing into the Fairness of Large Language Models: A Case Study of ChatGPT

- **Authors:** Yunqi Li, Lanjing Zhang, Yongfeng Zhang
- **Venue / year:** IEEE CISS 2024
- **Paper:** https://doi.org/10.1109/ciss59072.2024.10480206
- **Tags:** LLM Fairness, Bias Probing, ChatGPT, Evaluation

## Introduction

LLM 可能在性别、种族、职业、地区和社会群体问题上产生不平等输出；ChatGPT 的流畅性让这些偏差更难被用户察觉。本文用 case study 设计 fairness probes，测量模型在不同群体/情景下的差异和潜在 stereotype。

## Method / Framework

研究构造成对或对照 prompts，固定任务语义只改变 demographic attribute，收集 ChatGPT outputs，再按 bias/fairness criteria 进行人工或自动分析。评价包括 output sentiment/quality、stereotype、拒答/帮助程度和群体差异，旨在把“模型是否公平”转成可复现的 probe protocol。

## Baselines / Comparisons

主要对照是不同 demographic prompt、任务类型、措辞和可能的 prompt mitigation；公平性指标与不改变群体属性的 expected invariance 比较。当前工作区未获得 IEEE 全文，具体样本量/数值不在本笔记中虚构。

## Experiments / Findings

case study 说明 ChatGPT 的 fairness 不是单一模型级属性，而会随属性、语境、prompt phrasing 和任务变化；流畅、有礼貌的输出仍可能隐含 stereotype 或不对称帮助。公平性 probe 对模型更新、语言与文化高度敏感，应持续运行并结合人类审查。

## Ablation / Error Analysis

paired prompt、属性替换、temperature、评估器和任务类别是关键消融；如果不控制语义或只看平均分，会漏掉交互效应。自动 sentiment/LLM judge 可能不能识别隐含偏差，公平性还需要群体代表性、社会语境与利益相关者参与。

## Limitations

单一 ChatGPT case study、有限属性和人工/自动评分限制泛化；公平性定义具有规范性，不能只由模型输出统计决定。公开材料不足以逐表复核实验数字。

## 两句话总结

该研究用成对 demographic probes 检查 ChatGPT 在不同群体和任务上的输出差异，强调流畅和礼貌不等于公平。它提供了可重复的偏差检测思路，但属性覆盖、文化语境、评估规范和公开实验细节仍需扩展。

## Evidence note

基于 IEEE DOI/公开题录整理；全文访问受限，已明确标注未复核的具体指标。

