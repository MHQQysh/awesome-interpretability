# 430. A Stitch in Time Saves Nine: Detecting and Mitigating Hallucinations of LLMs by Validating Low-Confidence Generation

- **Authors:** Neeraj Varshney, Wenlin Yao, Hongming Zhang, Jianshu Chen, Dong Yu
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2307.03987
- **Tags:** `hallucination` `uncertainty` `validation` `web-search` `mitigation`

## Introduction
长回答中的早期 hallucination 会被后续句子引用并进一步放大，因此“生成完再检查”常常太晚。本文提出边生成边利用低置信度 token/concept 触发验证，在错误扩散前修正。

## Method / Framework
系统按 sentence/concept 逐步生成，用 logit probability 找 uncertain concepts；对可疑 concept 生成 validation question，使用 self-inquiry 或 web search 检查事实。若验证失败，就重写/纠正当前句子，再继续后续生成，形成 active detection and mitigation loop。

## Baselines / Comparisons
比较 vanilla GPT-3.5/text-davinci-003/Vicuna-13B、self-inquiry validation、web-search validation、句子级和 concept-level probability threshold，以及生成后一次性检测。指标是 recall/precision、hallucination ratio、true-positive mitigation success 和 false-positive 新错误。

## Experiments / Findings
低概率对 hallucinated concepts 提供 signal，web-search validation 的 detection recall 通常优于 self-inquiry；概念级识别比整句概率更细。作者报告方法将 GPT-3.5 hallucination 从约 47.4% 降低，并在 correctly detected cases 中约 57.6% 成功修复；多跳 HotpotQA/false-premise 实验显示逐步验证能减少错误传播。

## Ablation / Error Analysis
概率聚合方式、sentence vs concept、validation source、threshold 和是否 early exit 是主要消融。失败来自搜索结果不足、实体/日期部分纠错、修订后偏题和 validator 自身误判；过低 threshold 会造成成本和 false positives。

## Limitations
系统依赖实时 web search、额外 LLM calls 和人工/规则评测，延迟与隐私成本高。低置信度不等于错误，高置信度也不能排除 hallucination；开放式推理和无 web evidence 的回答仍难验证。

## 两句话总结
本文在生成过程中用低置信度 concept 触发 self-inquiry/web-search 验证并即时修正，以阻止 hallucination snowball。实验显示 recall 和部分修复率提升，但验证器、搜索源和阈值带来新的误报、延迟与覆盖限制。

## Evidence note
已读取本地 PDF 的 active validation pipeline、概率信号、web-search 对比、57.6% 修复率和 HotpotQA/false-premise 实验。
