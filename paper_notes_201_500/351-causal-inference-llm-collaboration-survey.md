# 351. Large Language Models and Causal Inference in Collaboration: A Survey

- **Authors:** Xiaoyu Liu, Paiheng Xu, Junda Wu, Jiaxin Yuan, Yifan Yang, Yuhang Zhou, Fuxiao Liu, Tianrui Guan, Haoliang Wang, Tong Yu, Julian McAuley, Wei Ai, Furong Huang
- **Venue / year:** arXiv survey, 2024/2025 revision
- **Paper:** https://arxiv.org/abs/2403.09606
- **Tags:** `causal-inference` `llm-interpretability` `causal-reasoning` `fairness` `multimodal`

## Introduction
The survey studies the relationship in both directions: causal inference can be used to understand and improve LLMs, while LLMs can supply world knowledge, textual priors, and counterfactual data for causal analysis. Its motivation is that next-token prediction can look like reasoning while still relying on spurious correlations, and that causal tools offer a stricter language for testing whether an intervention changes a model's behavior.

The paper organizes the LLM-side literature into reasoning capacity, fairness and bias, safety, explainability, multimodality, and causal benchmarks. The causal-inference-side literature is organized into treatment-effect estimation and causal-relationship discovery. It is therefore a map of a developing field rather than a new model or a single benchmark.

## Method / Framework
The authors first review Transformer LLMs, prompting, instruction tuning, and causal inference through potential outcomes, directed acyclic graphs, and structural equations. They then classify prior work according to whether causality is used to probe the model or to augment a downstream causal method.

For interpretability, the survey identifies three intervention families: input/prompt intervention, inner-component intervention, and causal-graph abstraction. Input intervention constructs counterfactual texts and observes output changes; component intervention exchanges or mediates activations in attention/MLP modules; graph abstraction searches for a small causal program that explains a behavior, as in DAS-style analyses. For causal discovery, LLMs are used as pairwise edge-direction judges or as priors/constraints that are combined with constraint-based, score-based, or iterative discovery algorithms.

## Baselines / Comparisons
Because this is a survey, the comparison unit is the prior-study design rather than a single baseline table. The text contrasts LLM judgments with expert knowledge, statistical causal discovery, supervised neural extraction, and conventional black-box probing. It also distinguishes textual and multimodal benchmarks and compares counterfactual, commonsense, model-understanding, and fairness/debiasing evaluations.

Representative evidence is mixed: some pairwise causal-direction studies report accuracy up to 97%, while other work finds prompt sensitivity, cycles when pairwise judgments are assembled into a graph, quadratic query cost, and confident false causal claims. The survey also contrasts causal interventions with ordinary correlation-based prompting, and code LLMs with text LLMs on abductive/counterfactual reasoning.

## Experiments / Findings
The main finding is not that LLMs are causal reasoners, but that causal methods expose where their apparent reasoning is fragile. Counterfactual augmentation can reduce spurious correlations on simpler sentiment/NLI tasks, yet generated counterfactuals become low quality for harder relation extraction. Prompt perturbations show that CoT can be causally influential while also tracking irrelevant properties such as sentence length.

For safety and fairness, causal graphs help identify proxy paths and motivate interventions on latent or intermediate variables. For explanation, mediation and activation interchange can locate components causally implicated in behavior, but the result depends on the chosen intervention and hypothesis class. In multimodal settings, language priors can dominate visual evidence; causal self-consistency and causal-enhanced agents are proposed to reduce this shortcut.

On the reverse direction, LLMs can provide causal orders, qualitative ancestral constraints, or textual priors for graph discovery, and can generate counterfactual examples for treatment-effect estimation. The reported benefit is strongest when the LLM is coupled with a data-driven method rather than trusted as a standalone oracle. The survey highlights that DISCO-like generation and LLM-guided initialization can fill data or knowledge gaps, but correctness still has to be checked by causal assumptions and consistency tests.

## Ablation / Error Analysis
The survey's error analysis is cross-paper: prompt wording changes causal judgments; independently judged edges can form cycles; pretraining frequency of causal mentions affects performance; and the model may confuse causal language with causal structure. These are failure modes of both the model and the evaluation protocol.

The most important ablation logic is to compare ordinary prompting with controlled interventions: change only a prompt feature, swap an inner activation, or remove a causal path. Such comparisons separate a verbalized rationale from a mechanism that actually changes the prediction. The authors repeatedly caution that a fluent causal explanation is not evidence that the model used the stated causal pathway.

## Limitations
This is a manually curated survey of a fast-moving intersection, so coverage is not exhaustive and papers can fit multiple categories. Reported accuracies are not directly comparable because tasks, prompts, model access, causal assumptions, and evaluation datasets differ. The survey also inherits the field's limitation that causal discovery from observational language is underidentified, while LLM-generated counterfactuals can introduce plausible but unsupported facts.

## 两句话总结
这篇综述把“大模型会不会因果推理”拆成了可检验的输入干预、组件干预、因果图抽象，以及用 LLM 辅助因果发现和效应估计两条路线。它的核心提醒是：LLM 可以提供有用的世界知识和候选因果结构，但必须用干预、一致性约束和传统因果方法验证，不能把流畅的 CoT 当成真实因果机制。

## Evidence note
本笔记依据 arXiv v3 全文逐节核对；论文是综述，文中的百分比和方法结论均属于被综述论文的报告，不是本文自己的新实验。
