# 146. Document-Level Machine Translation with Large Language Models

- **Authors:** Longyue Wang, Chenyang Lyu, Tianbo Ji, Zhirui Zhang, Dian Yu, Shuming Shi, Zhaopeng Tu
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.1036
- **Tags:** Machine Translation, Discourse, Long Context, Evaluation

## Introduction

句子级 MT 可能译得局部正确，却在整篇文档中出现实体不一致、代词错误、术语漂移和篇章不连贯。论文把 document-level translation 当作探针，研究 ChatGPT / GPT-4 是否真的利用长上下文和 discourse knowledge，而不只是生成流畅句子。

## Method / Framework

研究分三部分：比较 context-aware prompts；把 ChatGPT 与商业 MT 和先进 document-level NMT 比较；用 contrastive tests 检查 deixis、ellipsis、lexical consistency 和 zero pronoun 等 discourse phenomena。作者设计多个 prompt，比较只给当前句、给全文、按多轮上下文组织以及显式要求解释 discourse 决策的版本。

数据覆盖中英、英德、英俄和 news、social、fiction、Q&A、TED、Europarl、subtitle 等领域。指标包括 BLEU、document BLEU（d-BLEU）、COMET、zero-pronoun / anaphora accuracy、人工 general quality 与 discourse awareness（0 到 5）。

## Baselines / Comparisons

商业系统包括 Google Translate 等，神经 baseline 包括 MR-Doc2Doc 及其它 document-level NMT；LLM 比较 GPT-3.5、GPT-4 和不同训练变体。训练变体分析 code pretraining、SFT、RLHF 的潜在影响，但作者明确指出这些变体同时改变模型规模和数据，不能作严格因果归因。

## Experiments / Findings

- context-aware prompt 在 Chinese-English 上提高 document-level consistency，尤其是 terminology 和 zero-pronoun；但 prompt 间差异依赖领域和文档长度，fiction 与 TED 更难。
- GPT-3.5 / GPT-4 在 human general quality 和 discourse awareness 上超过商业 MT，但 d-BLEU 上商业系统有时更高，体现自动指标和人工质量的错位。
- ChatGPT 在长文档、文学内容和复杂指代上展现出强潜力，但 deixis、lexical consistency 等 contrastive tests 仍有明显错误；GPT-4 通常优于 GPT-3.5。
- GPT-4 的 explanation accuracy 与 prediction accuracy 不总相关：模型可能选对翻译却给出错误的 discourse 理由，说明自然语言解释不能直接作为内部知识的验证。
- 训练演化分析显示 code pretraining、SFT 和 RLHF 可能分别影响 discourse probing 与翻译质量，但结论只能作为观察，不是严格 ablation 因果。

## Ablation / Error Analysis

prompt 消融比较 P1、P2、P3 等上下文组织；P3 在多轮上下文和术语一致性上较好。文档长度、领域、语言对性能影响显著。错误集中在长文档信息选择、代词指代和自动指标无法反映自然度；人工评审中还可能有评审背景差异，作者使用专业译者并报告显著性检验。

## Limitations

GPT-3.5 / GPT-4 结果来自 2023 年 3 月 API，模型会更新，复现实验受到版本和不可见系统 prompt 影响。文档数量、语言和领域仍有限；人工协议虽覆盖 discourse，但不能全面代表真实翻译质量。不同训练技术的比较有混杂变量，且 d-BLEU、COMET 与人工偏好并不一致。

## 两句话总结

论文把 ChatGPT 作为 document-level MT 和 discourse modeling 的探针，发现它在人工整体质量和长上下文一致性上很强，但并不稳定地理解指代和篇章结构。它还警告，翻译结果正确与解释正确不是一回事，prompt、领域和评测指标都会改变结论。
