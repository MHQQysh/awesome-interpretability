# 321. FABLES: Evaluating Faithfulness and Content Selection in Book-Length Summarization

- **Authors:** Yekyung Kim, Yapei Chang, Marzena Karpinska, Aparna Garimella, Varun Manjunatha, Kyle Lo, Tanya Goyal, Mohit Iyyer
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2404.01261
- **Tags:** Summarization, Faithfulness, Long Context, Human Evaluation

## Introduction

长篇小说可超过 100K tokens，LLM 逐段总结后可能出现事件、人物状态和因果关系错误；短文本自动 factuality metric 不足以评估这些需要跨章节推理的 claim。FABLES 要解决的是如何建立没有训练数据污染、细粒度、人工核验的 book-length summary faithfulness benchmark。

## Method / Framework

作者选择 2023/2024 出版、模型不太可能见过的 26 本书，招募真正读过书的 14 名英语母语 annotators；用 hierarchical merging 让 GPT-3.5, GPT-4, GPT-4-Turbo, Mixtral 和 Claude-3-Opus 生成摘要，再把摘要拆成 atomic claims。人工逐 claim 标注 faithful/unfaithful 与内容选择/总体质量，形成 3,158 条 FABLES annotations，成本约 $5.2K。

## Baselines / Comparisons

比较五个 summarizer、不同书籍/长度和 LLM auto-rater；自动指标与人工 claim labels 对照。评价重点是 unfaithful claim detection、content selection 与整体 summary quality，而不是只用 ROUGE/BLEU 或短文 entailment。

## Experiments / Findings

Claude-3-Opus 在 faithfulness ranking 上显著优于其他闭源 LLM，开源 Mixtral 与 GPT-3.5-Turbo 接近。多数错误 claim 关于事件和人物状态，通常需要对叙事进行间接推理才能证伪；自动 LLM raters 与人工 faithfulness 相关性弱，尤其难识别 unfaithful claim。即使最好 auto-rater 在 unfaithful classification 只有约 58.2 F1，也不能替代人工核验。

## Ablation / Error Analysis

hierarchical chunking、claim extraction、书籍新颖性和 annotator 是否完整读书是关键设计；100 条人工核验的 claim extraction precision 为 100%。自动 judge 可能被摘要的流畅性和局部事实迷惑，跨章节因果/人物状态需要全书证据；按书/模型切分后小样本也会使分数不稳定。

## Limitations

人工阅读昂贵，26 本英文小说不能覆盖非虚构、其他语言和超长文档类型；作者已承认 per-model unfaithful score 样本有限。FABLES 评价摘要 faithfulness，不直接测用户决策或源文档引用质量。

## 两句话总结

FABLES 用新出版小说和读完整书的人工标注建立了 3,158 条长文摘要 claim-level faithfulness 数据，证明长篇叙事错误主要隐藏在事件、人物状态和间接因果中。它还显示当前 LLM auto-rater 对不忠实 claim 不可靠，长文摘要评价仍需要细粒度人工或更强验证器。

## Evidence note

本笔记逐段核对 arXiv PDF 的 dataset collection、summary generation、claim annotation、model comparison 与 auto-rater analysis。

