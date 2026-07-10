# 242. Explainable Artificial Intelligence (XAI): From Inherent Explainability to Large Language Models

- **Authors:** Fuseini Mumuni, Alhassan Mumuni
- **Venue:** arXiv 2025 survey
- **Paper:** https://arxiv.org/abs/2501.09967
- **Tags:** XAI Survey, LLM Explainability, Inherent Models, Post-hoc

## Introduction

黑盒模型在医疗、自动驾驶和高风险决策中难以让利益相关者理解或纠错；LLM 又把生成、推理和自然语言解释混在一起，使“解释”是否忠实更加复杂。综述从 inherent explainability 走到 post-hoc XAI 和 LLM explainability，整理方法、评价与开放问题。

## Method / Framework

文章按 taxonomy 讨论 inherently interpretable models、surrogate/feature attribution、gradient/perturbation、counterfactual、concept/rule、attention/probing 和 LLM-specific explanation，并梳理 local/global、model-agnostic/model-specific、intrinsic/post-hoc 的关系。重点还包括 faithfulness、fidelity、human comprehensibility、robustness 与 evaluation protocol。

## Baselines / Comparisons

比较 interpretable-by-design、black-box post-hoc 与 LLM-generated natural-language rationale，分析它们的透明度、表达力、计算成本、稳定性和因果保证。传统 XAI 指标与 LLM explanation 的 token/semantic evaluation 被放在统一风险框架中，而不是只按流畅度排序。

## Experiments / Findings

- 综述指出 inherent model 的解释来自结构，通常更稳定但表达能力有限；post-hoc 方法覆盖广却可能生成 plausible but unfaithful explanations。
- LLM 可把 feature/attribution 转成易读自然语言，但其解释会受 hallucination、prompt、模型偏见和 self-evaluation shortcut 影响；需要同时评价 explanation quality、decision faithfulness 与 user utility。
- 面向未来，统一 benchmark、因果干预、跨模型验证、隐私/公平与高风险领域部署是比再增加一种 visual rationale 更重要的方向。

## Ablation / Error Analysis

综述不做新 ablation；它归纳 gradient saturation、输入删改敏感性、attention 误读、surrogate mismatch 和 LLM rationalization 等错误来源。不同指标可能冲突，不能用单一 BLEU、human preference 或 attention score 代表 faithful explanation。

## Limitations

survey 覆盖面广而具体方法的最新实现、数据和结论仍需回到原论文核对；LLM 解释研究更新很快，taxonomy 可能迅速过时。没有统一 ground truth 也限制了跨任务比较。

## 两句话总结

综述把 XAI 从 inherent model、post-hoc attribution 延伸到 LLM natural-language explanation，并强调可读性、忠实度和因果性必须分开评价。它的主要价值是建立术语与风险地图，而不是提供一个单独的解释算法。
