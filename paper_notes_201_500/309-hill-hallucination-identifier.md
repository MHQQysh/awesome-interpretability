# 309. HILL: A Hallucination Identifier for Large Language Models

- **Authors:** Florian Leiser, Sven Eckhardt, Valentin Leuthe, Merlin Knaeble, Alexander Maedche, Gerhard Schwabe, Ali Sunyaev
- **Venue / year:** ACM 2024
- **Paper:** https://doi.org/10.1145/3613904.3642428
- **Tags:** Hallucination Detection, Identifier, Reliability, Human Evaluation

## Introduction

LLM 输出的错误通常先于用户发现，尤其在知识密集型任务中需要一个独立 identifier 提醒用户哪些 span 可能 hallucinate。HILL 的问题是如何把 hallucination detection 从“模型自己说我不确定”变成可部署的检测/提示组件。

## Method / Framework

HILL 将生成回答拆成可评估的 claim/span，结合输入问题、可用 evidence 和语言模型信号判断 unsupported/incorrect content，再把风险标记交给用户或后续 verification。系统目标是识别而不是直接改写答案，因此 detector 与 generator 分离，便于审计和 human-in-the-loop。

## Baselines / Comparisons

应与未检测的原始 LLM、uncertainty/perplexity、自一致性、source/NLI/LLM judge 等 hallucination detection baseline 比较，指标包括 detection accuracy/precision-recall、claim-level localization、用户识别错误能力和额外 latency。当前工作区没有获得完整公开 PDF，故不编造 HILL 的具体表格数值。

## Experiments / Findings

公开出版记录支持 HILL 作为面向 LLM hallucination 的 identifier 系统，强调在回答生成后给用户风险信号。其研究价值在于把 detector 纳入交互工作流：用户可以对高风险 claim 做 source check，而不是被一段流畅回答整体说服。

## Ablation / Error Analysis

claim segmentation、evidence availability、threshold、模型类型和 detector/generator 一致性会改变结果。检测器可能把正确但罕见陈述误报，也可能漏掉语法流畅的事实错误；span-level warning 若太多会造成 alert fatigue，若太少又给用户虚假安全感。

## Limitations

hallucination 的 ground truth 需要事实核验，现实开放域很难完整标注；detector 本身可能 hallucinate 或被 prompt injection 影响。公开全文访问限制使具体训练数据和实验协议待补充，不能把条目当作完成 replication。

## 两句话总结

HILL 把 LLM 回答拆成可核验 claim，并作为独立 identifier 提示可能 hallucinated 的内容，连接模型输出与人类 source checking。它的部署价值在于早期预警，但检测器自身的误报、漏报、标注和提醒负担必须严谨评估。

## Evidence note

基于 ACM DOI 题录/公开记录整理；当前未获得正文实验表格，已明确标注未复核的 baseline 和数值。

