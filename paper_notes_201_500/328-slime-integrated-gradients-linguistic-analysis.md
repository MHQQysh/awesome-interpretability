# 328. A Methodology for Explainable Large Language Models with Integrated Gradients and Linguistic Analysis in Text Classification

- **Authors:** Marina Ribeiro, Bárbara Luzia Covatti Malcorra, Natália Bezerra Mota, Rodrigo Wilkens, Aline Villavicencio, Lilian Cristine Hübner, César Rennó-Costa
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2410.00250
- **Tags:** SLIME, Integrated Gradients, Alzheimer Detection, Linguistic Analysis

## Introduction

从 speech transcription 识别 Alzheimer’s disease 需要模型捕捉微妙 lexical/social cues，但分类器不能告诉临床人员为何判断为 AD。SLIME 的问题是把 BERT 的 token attribution 与语言学统计结合，产生可理解、可验证的临床特征。

## Method / Framework

在 Cookie Theft picture-description transcriptions 上微调 BERT 做 AD/control 二分类，再用 Integrated Gradients 找影响预测的词。LIWC 与统计分析把高 attribution lexical components 映射到 social references、功能词等 linguistic categories，形成 model decision 的可读解释；数据有 156 narratives，AD/control 各 78。

## Baselines / Comparisons

比较 BERT 原始分类、不同 attribution/linguistic feature selection、统计语言指标与 integrated-gradient explanation；评价 classification accuracy/F1 与选出特征的临床/语言解释一致性。方法重点是解释增强而不是更换语言模型。

## Experiments / Findings

SLIME 发现 BERT 依赖能反映 AD 中 social references 减少的 lexical components，并能指出哪些词对预测最重要；语言学分析还能补充或改进分类特征。结果强调 attribution 与 domain linguistic knowledge 结合比单独显示 top tokens 更适合临床解释。

## Ablation / Error Analysis

IG baseline、LIWC categories、词汇统计、数据划分和 tokenization 是关键消融；小数据会让 attribution 不稳定，词的高重要性可能是患者话题/图像描述差异而非疾病因果。需要 age/sex、speaker、picture content controls 和 external clinical validation。

## Limitations

单一 Cookie Theft 任务、156 人和英语数据限制泛化；BERT attribution 仍是模型行为 proxy，不是神经语言机制。临床解释必须经过医生验证，不能直接用于诊断。

## 两句话总结

SLIME 用 Integrated Gradients 找 BERT 的关键词，再用 LIWC/统计语言分析把 AD 分类理由转成临床可读特征。它将 token attribution 与领域语言学结合，但小样本、任务偏差和解释非因果性仍需外部验证。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Cookie Theft dataset、BERT/IG/LIWC pipeline、results 和 limitations。

