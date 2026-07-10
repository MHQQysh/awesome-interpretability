# 349. Current state of LLM Risks and AI Guardrails

- **Authors:** Suriya Ganesh Ayyamperumal, Limin Ge
- **Venue / year:** arXiv 2024
- **Paper:** https://arxiv.org/abs/2406.12934
- **Tags:** `survey` `guardrails` `safety` `bias` `privacy`

## Introduction
The paper surveys risks that appear when LLMs are deployed in sensitive applications: bias and fairness failures, harmful or unsafe actions, poisoned data, hallucination, privacy leakage, poor explainability, and non-reproducibility. It asks how guardrails can constrain a probabilistic model while preserving useful flexibility, especially for agentic systems that can act in the world.

## Method / Framework
The organizing framework is a three-layer protection model. The **Gatekeeper layer** handles inputs/outputs through system prompts, instruction/few-shot prompting, self-consistency, prompt-injection defenses, fairness checks, and external response validation. The **Knowledge Anchor layer** grounds the model through RAG, vector similarity, corrective retrieval, and knowledge graphs. The **Parametric layer** changes the model through domain fine-tuning, specialist models, synthetic balancing, adversarial fine-tuning, and counterexample training.

The paper maps risks to mitigation strategies in a table. Hallucination is addressed by grounding and response validation; bias by counterbalanced examples, fairness-guided prompting, and fine-tuning; adversarial protection by hardened prompts, membership-inference defenses, tighter retrieval, and adversarial training; privacy/unknowability by external truth checks and counterexample methods.

## Baselines / Comparisons
This is a conceptual survey rather than an experimental benchmark. It compares external prompt/output guardrails, retrieval grounding, and parameter-level adaptation. It also discusses open-source tools: NeMo Guardrails uses the Colang DSL and LLM-based decisions, Llama Guard classifies input/output safety categories using a Llama-2-7B-derived model, and Guardrails AI uses the RAILS XML-style specification and validators.

The comparison exposes a defense-depth principle: gatekeeper controls are easy to deploy but can be bypassed; knowledge anchoring reduces hallucination but depends on retrieval quality; parameter changes can improve domain behavior but risk catastrophic forgetting and require retraining.

## Experiments / Findings
The survey's main finding is that guardrails are not a single filter. They must be layered across access, retrieval, model parameters, and post-generation validation. RAG can reduce dependence on memorized data and ground answers, but it does not eliminate prompt injection or guarantee that the generator follows retrieved evidence. Fine-tuning can reduce bias or toxicity, but it may introduce new behaviors and can be difficult to validate for an evolving model.

For agentic LLMs, the paper emphasizes testability, fail-safe behavior, and situational awareness. A system that can call tools or take actions needs stronger controls than a chatbot because an explanation after an unsafe action is not sufficient. Monitoring should include inputs, outputs, model/version changes, and the behavior of guardrail components themselves.

## Ablation / Error Analysis
There is no controlled ablation. The survey's failure analysis compares flexibility versus stability, emergent complexity, unclear objectives/metrics, testability/evolvability, and cost. Overly rigid guardrails can block benign use; weak guardrails expose bias, hallucination, and misuse. Adding multiple LLM judges can improve coverage but increases latency and cost, and judge models may be biased toward their own outputs.

The paper also points out a metric problem: cosine similarity and surface-level matching do not reliably establish semantic correctness. Guardrail validators need task-specific definitions of harm, fairness, grounding, and acceptable uncertainty rather than one generic score.

## Limitations
The authors explicitly note that this is a fast-moving field and some reviewed strategies may become obsolete or be disproven. The survey is broad but not a controlled evaluation of NeMo Guardrails, Llama Guard, or Guardrails AI. Several recommendations are conceptual, and real-world efficacy depends on threat models, data governance, deployment architecture, and human oversight.

## 两句话总结
这篇综述用 Gatekeeper、Knowledge Anchor、Parametric 三层框架整理 LLM 风险与 guardrail：提示/过滤、RAG/知识图谱、微调/专用模型分别承担不同防线。它强调 guardrail 不是单一安全滤网，真正的难点是灵活性与稳定性、组合系统测试、明确指标和成本之间的长期平衡。

## Evidence note
This note was written from the full survey PDF (arXiv:2406.12934v1), including the risk taxonomy, Table 1 layered-protection matrix, open-source tool descriptions, challenges, limitations, and conclusion.
