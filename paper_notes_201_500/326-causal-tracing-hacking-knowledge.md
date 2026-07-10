# 326. Causal Tracing to Identify Hacking Knowledge in Large Language Models

- **Authors:** Ryan Marinelli
- **Venue / year:** IEEE BigData 2024
- **Paper:** https://doi.org/10.1109/bigdata62323.2024.10825125
- **Tags:** Causal Tracing, Hacking Knowledge, Knowledge Localization, Cybersecurity

## Introduction

LLM 可以回答网络安全/黑客相关问题，但很难知道这些知识存在哪里、输出是否来自训练记忆、检索上下文还是推理组合。论文将 causal tracing 用于 hacking knowledge localization，希望定位对 cybersecurity responses 有因果贡献的 layers/neurons/representations。

## Method / Framework

方法构造 clean prompt 与 corrupted prompt，对 subject/entity/relation token 的 hidden states 做 patching，观察 answer logit recovery；再按 layer、MLP/attention 和 token position 聚合 causal effect。结果可以用来区分 factual knowledge storage 与 prompt-dependent computation，并为安全知识审计或编辑提供候选位置。

## Baselines / Comparisons

比较 clean/corrupted causal tracing、直接 probing/logit lens、不同 corruption 类型和不同 hacking/security questions；评价是 answer recovery、layer localization 和跨 prompt consistency，而不是网络入侵效果。公开 DOI 正文未能逐表获取，具体模型与数值不在此虚构。

## Experiments / Findings

论文的应用性 finding 是 causal tracing 能显示模型回答 hacking facts 时哪些中间 states/层被恢复后输出改变，提供比相关 probe 更强的 knowledge localization 证据。该信息可用于安全 red-team、knowledge editing 或识别模型是否依赖 memorized dangerous content。

## Ablation / Error Analysis

corruption position、patch layer、subject/object choice、答案 token 和 prompt paraphrase 是关键消融；错误定位可能来自 patching 把信息直接注入目标位置，或把语言模板效果误认为知识 storage。需要 random control、semantic-matched prompts 和 intervention strength sweep。

## Limitations

安全知识定义、数据污染、模型版本和 task wording 会强烈影响 localization；causal tracing 证明“恢复该 state 可恢复输出”，不等于该 state 是唯一存储位置。公开资料不足以确认完整实验和外部安全效用。

## 两句话总结

该工作把 causal tracing 应用于 hacking knowledge，利用 clean/corrupted activation patch 定位影响安全回答的层和表示。它比单纯 probing 更接近因果证据，但 patch injection、prompt 依赖和公开实验细节限制了结论强度。

## Evidence note

基于 IEEE DOI 题录/摘要与可获得描述整理；全文表格未获得，具体指标留待正式版本复核。

