# 303. On the Limitations of Large Language Models: False Attribution

- **Authors:** Tosin Adewumi, Nudrat Habib, Lama Alkhaled, Elisa Barney
- **Venue / year:** 2024/2025 preprint
- **Paper:** https://arxiv.org/abs/2404.04631
- **Tags:** Hallucination, False Attribution, SHI, Author Identification

## Introduction

LLM hallucination 不只表现为虚构事实，也包括把一段文字错误归因给错误作者。小文本片段缺少显著风格线索时，模型可能用 parametric memory 猜测并高置信度输出。论文把 false attribution 形式化为一类 hallucination，并提出 Simple Hallucination Index (SHI) 衡量风险。

## Method / Framework

研究让 LLM 对短文本做 author attribution，并比较模型回答、真实作者和 prompt/context 变化；SHI 将错误归因行为汇总为简单、可比较的 hallucination 指标。实验关注模型是否承认不确定、是否把不充分证据当成记忆，以及不同 prompt/文本长度对错误的影响。

## Baselines / Comparisons

比较不同 LLM、prompt 设计和文本 chunk/作者候选设置；baseline 是正确 attribution、拒答/uncertain response 和直接生成作者名。指标包括 attribution accuracy 与 SHI，不把语言流畅度或 confidence 当成正确性。

## Experiments / Findings

论文展示 Mixtral 等模型在短文学片段上会把作者错误归因给知名作家，且输出形式看似确定；SHI 能把这类 false attribution 从一般文本错误中单独监测。文本越短、候选越模糊、训练记忆越强时，模型更容易用熟悉作者替代真实证据。

## Ablation / Error Analysis

chunk length、prompt 是否要求 evidence/uncertainty、候选列表和模型规模是关键对照；模型有时正确识别文本但错误生成作者解释，说明 attribution rationale 可能是 post-hoc。SHI 依赖 ground truth 作者和任务定义，不能覆盖所有 factual hallucination。

## Limitations

作者归因是窄任务，文学语料和模型训练污染会影响结果；SHI 是行为指标，不能证明内部 hallucination mechanism。真实知识任务还需要 source verification、retrieval 和 human adjudication。

## 两句话总结

该工作把“错误作者归因”作为 LLM hallucination 的具体、可测形式，提出 SHI 监控短文本证据不足时的错误确定性。它提醒我们模型可能用熟悉记忆替代证据，且自然语言解释不能自动修复 attribution error。

## Evidence note

本笔记逐段核对 arXiv PDF 的 false-attribution task、SHI definition、prompt examples 和 limitations；没有把示例错误率扩展成未经复核的普遍结论。

