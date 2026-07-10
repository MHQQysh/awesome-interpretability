# 308. A Comprehensive Survey of Hallucination Mitigation Techniques in Large Language Models

- **Authors:** S. M. Towhidul Islam Tonmoy, S. M. Mehedi Zaman, Vinija Jain, Anku Rani, Vipula Rawte, Aman Chadha, Amitava Das
- **Venue / year:** 2024 survey
- **Paper:** https://arxiv.org/abs/2401.01313
- **Tags:** Hallucination Survey, Detection, Mitigation, Evaluation

## Introduction

LLM 会生成看似事实但无依据的内容，成为真实部署的主要障碍。本文整理 hallucination 的定义、来源、类型、检测和 mitigation，重点说明不同阶段（data/model/inference/post-generation）能采取什么办法，以及如何评估减少 hallucination 是否真的有效。

## Method / Framework

survey 按 hallucination cause/taxonomy、detection、mitigation 和 evaluation 组织文献：pretraining/data cleaning、alignment/fine-tuning、retrieval-augmented generation、prompt/self-consistency、post-hoc verification、tool use 和 human feedback。评价维度包括 factuality/groundedness、faithfulness、coverage、fluency、consistency、cost 与 robustness。

## Baselines / Comparisons

不同 mitigation 方法按是否需要额外训练、外部知识、模型内部 access、推理成本和可扩展性比较；detector 包括 uncertainty/perplexity、self-consistency、retrieval/source checking、NLI/LLM judge。survey 反对只用一个 benchmark/automatic score，因为 factuality 与 fluency 可能互相牵制。

## Experiments / Findings

综述总结没有单一方法适合所有 hallucination：RAG 能补事实但会产生 context conflict/引用错误，fine-tuning 能改善领域行为但可能造成新偏差，prompt/CoT 改善某些 reasoning 任务却不保证 factual grounding，post-hoc verification 增加成本。未来需要统一 taxonomy、可复现基准、长尾/多语言/多模态测试和 calibration。

## Ablation / Error Analysis

应区分 intrinsic knowledge error、fabrication、contradiction、irrelevance 与 omission；否则某方法在一个 hallucination 定义上胜出并不代表普遍更可靠。评价还要做 source corruption、retrieval quality、model size、temperature 和 domain shift 消融，防止 judge model 把语言流畅误判为事实。

## Limitations

survey 覆盖文献的任务、指标和定义不统一，不能直接计算跨论文平均提升；开放域 factuality 与用户信任/安全结果之间仍有 gap。hallucination mitigation 还会带来 latency、API cost、privacy 和 over-refusal。

## 两句话总结

该 survey 把 hallucination 按原因、检测与 mitigation 阶段系统归类，强调 RAG、训练、prompt、verification 各自只能覆盖部分失败模式。它的核心结论是需要 groundedness、faithfulness、robustness 和成本的联合评价，而不是追逐单一 hallucination 分数。

## Evidence note

本笔记逐段核对 survey 的 taxonomy、detection/mitigation categories、evaluation metrics、limitations 和 future directions。

