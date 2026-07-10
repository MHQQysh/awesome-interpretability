# 298. Probing the Consistency of Situational Information Extraction with Large Language Models: A Case Study on Crisis Computing

- **Authors:** Andrea Salfinger, Lauro Snidaro
- **Venue / year:** IEEE CogSIMA 2024
- **Paper:** https://doi.org/10.1109/cogsima61085.2024.10553903
- **Tags:** Information Extraction, Crisis Computing, Consistency, LLM Evaluation

## Introduction

危机计算需要从社交媒体/事件文本中提取地点、时间、伤亡、需求和行动等 situational information；LLM 的自然语言能力有潜力降低标注成本，但同一事实换个 prompt 或重复请求可能得到不一致结构。论文关注可解释的 extraction consistency，而不仅是一次运行的 F1。

## Method / Framework

研究把 crisis text 交给 LLM 做 structured information extraction，比较不同提示/输入表达下实体、关系和事件字段是否稳定，并对不一致案例做 qualitative analysis。解释性体现在输出字段、证据片段与 schema 可检查，而不是让模型写任意 rationale；一致性成为对 deployment reliability 的主要诊断。

## Baselines / Comparisons

比较不同 prompting/LLM settings 与传统信息抽取或人工标注参考，评价 extraction correctness、field-level consistency、missing/spurious facts 和可用性。当前工作区未获得 IEEE 正文全文，无法逐项复核模型名称、数据集规模和统计表，以下不写未经核对数字。

## Experiments / Findings

公开题录与研究主题支持的核心 finding 是：LLM 在危机信息抽取中可能生成语义上合理但 schema/字段不一致的结果，重复或改写 prompt 是发现这种不稳定性的有效方式。对 crisis response 来说，稳定性、缺失关键字段和误报同样重要，单次高质量样例不能证明系统可靠。

## Ablation / Error Analysis

prompt wording、temperature、schema strictness、few-shot examples 和事件类型是关键消融；实体边界、时间表达、地点歧义与事件状态会导致跨运行冲突。LLM 可能把推测写成事实，或遗漏低频但关键需求，因此需要 evidence span、人工 adjudication 和 temporal consistency 检查。

## Limitations

危机数据有语言、地区和事件分布偏差，模型输出也可能泄露敏感位置/个人信息；consistency 不等于 factual correctness。公开版本访问限制使具体实验细节待补充，不能把此条目当作完整 replication report。

## 两句话总结

该研究把危机计算中的 LLM 信息抽取从“单次准确率”推进到字段、证据和不同提示下的一致性检查。它强调在高风险事件响应中，稳定、可审计的结构化输出与 F1 同等重要，但具体模型和数值需以正式全文再核对。

## Evidence note

基于 IEEE DOI 题录/摘要与本地元数据整理；全文未获得，已显式标注未复核的实验细节。

