# 060. The Eval4NLP 2023 Shared Task on Prompting Large Language Models as Explainable Metrics

- **作者 / venue**：Eval4NLP Shared Task 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.eval4nlp-1.10/)

## Introduction 与任务

传统自动评价指标如 BLEU、ROUGE、BERTScore 往往不能完整反映 relevance、factuality 和 human preference。共享任务研究能否通过 prompt 让 LLM 充当 explainable metric，同时输出分数和理由。

## Baseline 与对比

基线包括 BERTScore、SBERT cosine similarity、SUPERT、BLEURT/BARTScore 等 embedding/likelihood metrics。参赛系统比较 prompt wording、few-shot examples、评分维度和自然语言 explanation；另有小规模 human evaluation 评估理由的 plausibility。

## Findings

LLM-based metrics 能捕捉部分传统 n-gram/embedding 指标忽略的语义和事实性，但分数对 prompt、示例、模型版本和输出格式敏感。自然语言 explanation 可以提高可读性，却不自动保证评分与人类判断一致。

## 局限

共享任务的 test set、prompt 和评测模型可能存在污染；解释质量和 metric correlation 需要分别评估。部署时还应报告 calibration、跨域稳定性和成本。

**一句话评价**：该共享任务把 LLM 评价器从“给一个分数”扩展到“给分数并解释”，但也暴露了 prompt-dependent metric 的稳定性问题。
