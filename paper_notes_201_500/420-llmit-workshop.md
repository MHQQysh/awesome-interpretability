# 420. Workshop On Large Language Models' Interpretability and Trustworthiness (LLMIT)

- **Authors:** Tulika Saha, Debasis Ganguly, Sriparna Saha, Prasenjit Mitra
- **Venue / year:** ACM workshop, 2023
- **Paper:** https://doi.org/10.1145/3583780.3615311
- **Tags:** `workshop` `interpretability` `trustworthiness` `LLM` `hallucination`

## Introduction
这篇 workshop/position 条目讨论大模型规模扩展带来的 opacity、emergence、hallucination 和 trustworthiness 问题，强调当模型从百万参数扩展到数十亿参数时，表面行为能力的提升不等于内部机制可理解。

## Method / Framework
文章围绕 LLMIT 议题组织 interpretability、trust、emergent capability、bias、factuality 和 evaluation，提出需要把 probing、rationale、attribution、model comparison 与 human-centered trust 放在一起，而非只追求 benchmark 分数。

## Baselines / Comparisons
主要比较 black-box behavioral evaluation、传统 NLP metrics、解释性/可视化方法、模型内部 probing 和 human evaluation。它是议题综述，不是一个新模型或统一 benchmark。

## Experiments / Findings
论文的中心 finding 是可解释性与 trustworthiness 相互关联但不等价：模型可能表现强却不能解释，解释文本也可能不忠实；emergent abilities 和 hallucination 需要更细粒度的机制、校准和风险评估。

## Ablation / Error Analysis
文章建议比较模型规模、prompt、任务和解释方法，并检查 rationale 是否随输入干预变化、模型错误是否能被解释提前预测。由于是 workshop 条目，没有完整的参数消融或统一错误表。

## Limitations
全文定位为 workshop/观点型工作，结论多为研究议程，实验和方法细节有限；不应把它当成某个可复现解释算法的实证论文。

## 两句话总结
LLMIT 条目把 LLM 的规模化能力、黑箱机制、hallucination 和用户 trust 放进同一可解释性议题中。它的价值是提出跨方法研究 agenda，但缺少统一实验，需与后续机制/评测论文配合阅读。

## Evidence note
出版商全文在当前环境不可直接获取；已核对 DOI/README 元数据和清单摘要，明确记录 workshop 文章的证据边界。
