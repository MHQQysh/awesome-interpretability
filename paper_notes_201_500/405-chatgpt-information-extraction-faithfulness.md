# 405. Evaluating ChatGPT's Information Extraction Capabilities: Performance, Explainability, Calibration, and Faithfulness

- **Authors:** Bo Li, Gexiang Fang, Yang Yang, Quansen Wang, Wei Ye, Wen Zhao, Shikun Zhang
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2304.11633
- **Tags:** `ChatGPT` `information-extraction` `explainability` `calibration` `faithfulness`

## Introduction
ChatGPT 能以少样本/零样本方式做实体、关系和事件抽取，但开放式文本输出带来格式错误、幻觉、置信度不校准和理由不忠实。本文不只问抽取 F1，还系统评估性能、解释质量、calibration 和 faithfulness，试图回答模型的自然语言 rationale 是否真的能帮助验证结构化信息。

## Method / Framework
实验将 IE 任务转成 prompt-based generation，要求 ChatGPT 输出抽取结果、可能的解释/依据和 confidence。作者比较 zero-shot、few-shot、不同 instruction/template，并设计 consistency/faithfulness 检查：改变输入、移除证据或比较 rationale 与 gold span，观察输出是否随真实证据变化。

## Baselines / Comparisons
基线包括传统监督 IE 模型、预训练 encoder/seq2seq 方法、zero-shot/few-shot ChatGPT 配置和人工/标注 gold。指标涵盖 precision/recall/F1、格式/结构合法率、calibration metrics、解释可读性和 evidence faithfulness。

## Experiments / Findings
论文显示 ChatGPT 在部分 IE 场景具有很强的 zero/few-shot 能力，但性能依赖 prompt、任务类型和标签格式；它可能生成看似合理但不在原文中的实体/关系。解释层能增加可读性和审计线索，却不能自动保证 faithful，confidence 也可能与真实正确率不匹配。

## Ablation / Error Analysis
主要消融是 prompt examples、输出格式、任务类型和是否要求 explanation/confidence。错误包括边界 span 错位、关系方向颠倒、遗漏多实体、格式不稳定和 rationale 引入未出现的事实。作者建议将结构化校验、校准和 evidence matching 作为使用 ChatGPT 做 IE 的必要后处理。

## Limitations
模型版本和 API 采样造成复现漂移，人工解释评分也有主观性；不同 IE 数据集的 label schema 不一致。faithfulness 的定义依赖 gold evidence，可能低估了合理但未标注的解释。ChatGPT 的高层输出评估不能替代对内部机制的分析。

## 两句话总结
本文把 ChatGPT 信息抽取的 F1、格式、置信度和自然语言解释放在同一个评测框架中，发现强行为表现并不自动带来校准和 faithful rationale。对实际应用而言，原文证据匹配、结构化约束和 calibration 必须与解释一起使用。

## Evidence note
已读取本地 PDF 的摘要、任务设定、解释/校准/faithfulness 评价和 prompt/error analysis；没有将跨数据集的结果混成单一数字。
