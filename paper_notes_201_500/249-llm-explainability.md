# 249. LLM Explainability

- **Authors:** Book chapter metadata entry
- **Venue:** Springer 2026 book chapter
- **Paper:** https://doi.org/10.1007/978-981-97-8440-0_85-1
- **Tags:** LLM Explainability, XAI, Natural Language Explanation

## Introduction

LLM 的规模、生成性和上下文依赖使传统 feature attribution 难以直接说明“为什么生成这句话”；用户又需要可读、可验证的理由。该章从 XAI 与 human-centered transparency 出发，讨论 LLM explanation 的技术路线和可信使用边界。

## Method / Framework

章节框架通常将 LLM explainability 分为 input/attention attribution、probing/representation、rationale/self-explanation、counterfactual/perturbation、mechanistic interpretability 和 external evidence grounding，并区分 intrinsic 与 post-hoc explanation。核心流程是从输出回溯输入/内部状态，再用自然语言或结构化证据呈现。

## Baselines / Comparisons

比较 attention/gradient/saliency、LLM-generated rationale、probe/SAE feature 和 causal intervention，维度包括 faithfulness、可读性、稳定性、计算开销和用户效用。章节强调 fluent rationale 与真实 decision process 之间需要单独测试。

## Experiments / Findings

- LLM explanation 的表达力高于 token highlight，但 self-explanation 可能是事后合理化；mechanistic/causal 方法更接近内部证据，却更难扩展和呈现。
- 可信解释需要多证据交叉：行为反事实、输入扰动、内部 feature 和外部 source 互相验证比单一自然语言理由更可靠。
- 高风险场景需要把解释当作辅助审计接口而非责任转移，尤其要报告不确定性、失败类型和解释覆盖范围。

## Ablation / Error Analysis

本条为章节型综述，未形成统一新 ablation；核心错误包括 prompt-dependent rationale、attention misinterpretation、feature label bias、counterfactual off-manifold 和用户把流畅理由当作事实。

## Limitations

章节对快速变化的 SAE、circuit tracing 与 multimodal LLM 进展可能不完整；不同解释方法没有统一 ground truth。具体工程选择仍需回到原论文和目标模型验证。

## 两句话总结

本章把 LLM explainability 放在 XAI、自然语言理由、内部 feature 与 causal intervention 的连续谱中，强调可读性不能替代 faithfulness。它适合作为概念地图，但不提供一个可直接解决所有 LLM 解释问题的算法。
