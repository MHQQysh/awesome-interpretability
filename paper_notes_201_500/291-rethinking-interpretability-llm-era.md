# 291. Rethinking Interpretability in the Era of Large Language Models

- **Authors:** Chandan Singh, Jeevana Priya Inala, Michel Galley, Rich Caruana, Jianfeng Gao
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2402.01761
- **Tags:** LLM Explainability, Natural Language Explanations, Interpretability Taxonomy

## Introduction

传统 interpretable ML 主要围绕 feature importance、可视化、可解释模型和 post-hoc distillation；LLM 的自然语言生成能力提供了把复杂模式转成语言解释的新机会，也带来 hallucinated explanation 和过度信任。本文重新思考 LLM 时代“interpretability”究竟是帮助理解、调试、控制，还是服务最终用户。

## Method / Framework

文章从 explanation 的对象与受众出发，区分 intrinsic/interpretable model、post-hoc explanation、self-explanation、feature attribution、model editing/steering 与 human-centered evaluation。它讨论 LLM 如何用语言把 feature、decision rationale、uncertainty 与 counterfactual 组织起来，并强调 explanation quality 应包含 correctness、faithfulness、usefulness、calibration 和用户可理解性。

## Baselines / Comparisons

这是概念/综述论文，不做单一模型 benchmark；比较传统 XAI、深度模型 post-hoc 方法、LLM 自解释和 mechanistic/causal interpretability 的目标与证据强度。尤其区分“对人看起来合理”与“真实反映模型内部决策”，并讨论不同受众需要不同解释粒度。

## Experiments / Findings

主要 finding 是 LLM 解释不应只被当作最终产品文案：它可以扩大模式表达、辅助 model debugging、数据分析和科学发现，但 fluent self-explanation 可能是 post-hoc rationalization。LLM 时代的机会是把解释生成、交互式追问和可执行干预结合，风险是模型自己生成的理由缺少独立验证。

## Ablation / Error Analysis

解释对象、prompt、模型能力和评价者都会改变结论；若只测 plausibility，容易奖励语言流畅而忽略 faithfulness；若只测 faithfulness，又可能不适合用户任务。文章倡导结合 mechanistic evidence、counterfactual tests、human studies 和 task outcomes，避免一个解释分数包打天下。

## Limitations

作为 perspective，本文提供问题框架而非统一算法/数据集；许多建议仍需要在真实 LLM 和高风险场景中验证。解释、透明、可控、可审计之间的概念边界也可能随应用变化。

## 两句话总结

这篇文章主张 LLM 时代的 interpretability 应同时服务理解、控制、调试和用户决策，而不能把自然语言 self-explanation 当作自动可信证据。它把传统 XAI、机制解释和 human-centered evaluation 放在同一框架下，强调 faithfulness 与 usefulness 的共同验证。

## Evidence note

本笔记逐段核对 arXiv PDF 的 taxonomy、LLM explanation opportunities/risks 与 evaluation discussion；没有将 perspective 观点写成新实验结果。

