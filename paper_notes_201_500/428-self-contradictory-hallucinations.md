# 428. Self-Contradictory Hallucinations of LLMs: Evaluation, Detection and Mitigation

- **Authors:** Niels Mündler, Jingxuan He, Slobodan Jenko, Martin Vechev
- **Venue / year:** ICLR, 2024
- **Paper:** https://arxiv.org/abs/2305.15852
- **Tags:** `hallucination` `self-consistency` `detection` `mitigation`

## Introduction
LLM 可能在同一问题的回答中生成互相矛盾的 claims，或在重复采样中给出不一致答案。self-contradiction 是一种可自动检测的 hallucination signal，尤其适合没有 reference answer 的开放式生成。

## Method / Framework
论文生成多次回答，抽取 claims 并用 NLI/semantic entailment 判断回答之间是否互相支持、矛盾或无关。检测结果再用于筛选、重采样、让模型修订或对用户提示不确定性。

## Baselines / Comparisons
比较单次回答、普通 self-consistency/majority vote、NLI contradiction detector、外部 factuality checker 和 mitigation variants。评价检测 precision/recall/AUROC、最终 factuality、contradiction rate 和 utility。

## Experiments / Findings
内部矛盾与错误回答相关，但不是所有 contradiction 都是 hallucination：合理的条件分支、时间变化和多观点也可能被 NLI 标成冲突。多次采样和 claim-level comparison 能发现单次阅读不易察觉的错误，并为修订提供反馈。

## Ablation / Error Analysis
采样数量、temperature、claim segmentation、NLI threshold 和修订次数是主要消融。过多采样增加成本和 false contradictions；过少采样漏掉低概率错误。NLI 受语言风格、否定、实体消歧和长句影响。

## Limitations
方法依赖可靠 claim extraction/NLI，无法发现所有 factual hallucinations；真实世界知识变化和多源冲突需要时间戳/来源建模。修订模型可能把检测结果变成新的 hallucination。

## 两句话总结
本文把多次回答中的 claim contradiction 作为无 reference hallucination signal，并研究检测、筛选和修订流程。self-contradiction 有用但不等于事实错误，必须结合语义、来源和时间条件避免过度拒答。

## Evidence note
已读取 ICLR 2024 本地 PDF 的 self-contradiction 定义、NLI 检测、采样/修订消融和误报讨论；README 中 arXiv 号不完整。
