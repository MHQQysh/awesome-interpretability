# 422. Minimizing Factual Inconsistency and Hallucination in Large Language Models

- **Authors:** I. Muneeswaran, Shreya Saxena, Siva Prasad, M. Prakash, Advaith Shankar, V. Varun, Vishal Vaddina, Saisubramaniam Gopalakrishnan
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2311.13878
- **Tags:** `hallucination` `factuality` `verification` `LLM`

## Introduction
医疗、教育、金融等高风险生成场景要求输出与事实和上下文一致，但 LLM 会生成表面流畅却错误的内容。本文研究如何通过生成后 factual consistency 检查和反馈，减少 hallucination，而不是仅依赖训练数据扩大。

## Method / Framework
方法将回答拆为 claims/concepts，使用模型概率/不确定性筛选可疑内容，再通过知识检索、验证问题和反馈提示检查/修改。生成、验证、修正可以循环，目标是保留流畅性同时提高 factual consistency。

## Baselines / Comparisons
比较 vanilla LLM generation、self-inquiry validation、web-search validation、不同 probability threshold 和后处理 mitigation。评价包括 hallucination detection recall/precision、成功修复率、false positives 和生成质量。

## Experiments / Findings
在 GPT-3.5/text-davinci-003 生成主题文章和多跳问答上，低 logit probability 对可疑 concept 提供信号；web-search validation 通常比直接 self-inquiry 有更高 detection recall。方法报告约 57.6% 的 correctly detected hallucinations 被成功修复，并把生成文本 hallucination 比例从约 47.4% 降低到更低水平；作者强调误报修正不会系统性引入新 hallucination。

## Ablation / Error Analysis
sentence-level vs concept-level validation、self-inquiry vs web search、minimum/average probability 和 sequential early exit 是主要消融。错误包括 web source 不完整、事实只部分错误、修复后偏离原问题、false positive 触发不必要改写，以及多跳问题仍有未验证桥接事实。

## Limitations
web search 的质量、成本和时效性限制实际部署；validator 也可能错误判断，且“修复成功”依赖人工/规则标注。系统主要适合可检索 factual claims，不覆盖主观写作、开放推理和内部机制解释。

## 两句话总结
本文用 token uncertainty 筛选可疑 claims，再通过 self-inquiry/web search 验证并逐句修正，实证显示能检测高比例 hallucination 并修复其中一部分。它把 factuality 从一次生成变成生成-验证-修正循环，但验证器和外部知识源仍是新的错误与成本来源。

## Evidence note
已读取本地 PDF 的概率筛选、self-inquiry/web-search validation、检测表、57.6% 修复率和失败案例。
