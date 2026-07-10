# 319. Leveraging Fuzzy Fingerprints from Large Language Models for Authorship Attribution

- **Authors:** Rui Ribeiro, João Paulo Carvalho, Luísa Coheur
- **Venue / year:** IEEE FUZZ 2024
- **Paper:** https://doi.org/10.1109/fuzz-ieee60900.2024.10612177
- **Tags:** Authorship Attribution, Fuzzy Fingerprints, LLM, Explainability

## Introduction

作者归因需要从语言风格判断文本来源，短文本、风格变化和 LLM 生成会增加不确定性。论文探索用 LLM 提取/描述作者风格的 fuzzy fingerprints，把硬标签 attribution 变成带程度和解释的模糊特征。

## Method / Framework

方法从文本生成或抽取作者 style fingerprints（词汇、句法、标点、语气等），以 fuzzy membership 表示文本对不同作者特征的归属程度，再进行 attribution。LLM 用于把复杂风格模式语义化，fuzzy aggregation 保留“像某作者但不确定”的程度。

## Baselines / Comparisons

比较传统 stylometry、硬分类、LLM direct attribution 与 fuzzy fingerprint aggregation，评价 attribution accuracy、uncertainty/interpretability 和不同文本长度/作者条件下的稳定性。公开 DOI 正文不可完整复核，具体数字不在本笔记中编造。

## Experiments / Findings

fuzzy fingerprints 的优势在于输出可解释的部分相似度而非只给作者名，适合短文本和风格混合；LLM 能帮助发现高层风格特征，但 attribution 仍会受到主题、文本长度和模仿风格影响。

## Ablation / Error Analysis

fingerprint vocabulary、membership function、文本长度、主题控制和 LLM prompt 是关键消融；如果主题词被当成风格，模型会把 content attribution 误当 authorship。需要 topic-matched、cross-domain 和 human stylistic validation。

## Limitations

作者风格不是静态唯一 fingerprint，LLM 生成文本、协作写作和风格模仿会打破闭集假设；fuzzy score 的校准与法律证据效力仍需谨慎。公开材料不足以确认完整 benchmark。

## 两句话总结

该研究用 fuzzy fingerprints 表示文本对作者风格特征的程度归属，让作者归因从硬标签变成可解释的不确定判断。它有助于处理短文本和风格混合，但主题泄漏、风格漂移和证据效力限制实际应用。

## Evidence note

基于 IEEE DOI/公开记录整理；全文实验表格未获得，具体数值留待正式版本复核。

