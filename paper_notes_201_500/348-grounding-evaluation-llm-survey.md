# 348. Grounding and Evaluation for Large Language Models: Practical Challenges and Lessons Learned

- **Authors:** Krishnaram Kenthapadi, Mehrnoosh Sameki, Ankur Taly
- **Venue / year:** KDD 2024 tutorial/survey
- **Paper:** https://arxiv.org/abs/2407.12858
- **Tags:** `survey` `grounding` `evaluation` `responsible-ai` `hallucination` `robustness`

## Introduction
For high-stakes LLM systems, accuracy alone is not enough. Users need responses grounded in evidence, robust to prompt and distribution changes, fair, secure, private, calibrated, and observable after deployment. This survey, accompanying a KDD tutorial, organizes practical challenges in building and evaluating generative AI systems and treats grounding as stronger than merely avoiding a hallucination: claims should be attributable to a relevant knowledge source.

## Method / Framework
The survey is organized around responsible-AI dimensions and a deployment lifecycle. It discusses hallucination/factuality, toxicity and harmful content, bias/fairness, robustness/security, privacy/unlearning/copyright, calibration/confidence, interpretability, grounding, and monitoring. For each dimension it separates business problems, technical approaches, evaluation metrics, and open challenges.

Grounding is treated as a pipeline: retrieve or supply context, generate a response conditioned on that context, verify that each claim is supported, and revise or reject the response when grounding fails. The survey reviews RAG, fine-tuning on context-response triples, NLI-based groundedness checks, attribution/citation systems, iterative revision, model-based evaluators, and human-in-the-loop workflows. It also discusses mechanistic interpretability and activation/weight statistics as possible monitoring signals.

## Baselines / Comparisons
There is no single benchmark baseline. The survey compares retrieval-based grounding with parametric-memory-only generation, automatic LLM/NLI judges with human evaluation, model-based safety filters with external guardrails, and static pre-deployment tests with continuous deployment monitoring. It highlights tools such as Eleuther Harness, LangTest, Fiddler Auditor, RAGAS, ARES, and other evaluation/guardrail systems as practical alternatives rather than ranking them.

The comparison is conceptual but useful: a response can be factually correct without being grounded in the supplied source, and a grounded response can still be incomplete, biased, or harmful. Evaluation therefore needs multiple axes and task-specific thresholds.

## Experiments / Findings
The survey identifies recurrent failure modes. RAG can retrieve irrelevant or incomplete passages; the generator may ignore or contradict the context; temporal information can be misunderstood; and NLI-based groundedness checks can fail on long or domain-specific claims. Stronger grounding may also reduce helpfulness or completeness if the system refuses to use relevant parametric knowledge.

For bias and fairness, the paper distinguishes intrinsic association tests from extrinsic task behavior and notes that mitigation can introduce new disparities. For robustness/security, it emphasizes minor prompt perturbations, jailbreaks, natural distribution shifts, and composed systems where one model's output becomes another model's input. For privacy/copyright, it discusses PII detection, data extraction, unlearning, and watermarking.

Calibration and confidence are treated as first-class explanation signals. Selective prediction, self-evaluation, semantic uncertainty, and confidence-aware deferral can tell the system when to ask for human review, but these methods themselves need adversarial robustness tests. The survey's overall finding is that trustworthy LLM evaluation must continue after deployment; a static benchmark cannot capture changing retrieval stores, prompts, users, or model versions.

## Ablation / Error Analysis
The survey's main error analysis is the distinction between factuality, groundedness, and explanation faithfulness. Citation presence does not guarantee that a citation supports the claim. LLM-as-a-judge can correlate with human judgments in some settings but can also inherit the model's biases, miss subtle contradictions, or reward fluent unsupported text.

The authors also warn that guardrails can be attacked and that adding more scaffolding creates emergent complexity. An evaluation suite should test the complete composed system, including retriever, prompt builder, generator, verifier, fallback, and user interface, rather than only the base model.

## Limitations
This is a broad practical survey with heterogeneous evidence, not a controlled meta-analysis. Metrics and definitions vary across domains, and some evaluation tools are themselves rapidly evolving. The survey cannot provide one universal grounding score or prove that a model is safe under all adversarial conditions. Human review remains expensive, while automated evaluation can be gamed.

## 两句话总结
这篇综述把 LLM grounding 定义为“每个回答主张都能被相关证据支持”，并把它放在 hallucination、bias、security、privacy、calibration、interpretability 与持续监控的完整评估框架中。核心经验是 RAG、NLI/LLM judge 或 citation 任一单点都不够，必须对整个 composed system 做多维、持续、带人工回退的测试。

## Evidence note
This note was written from the KDD 2024 survey PDF (arXiv:2407.12858), including the responsible-AI dimensions, grounding pipeline, evaluation/guardrail discussion, and open-challenge sections.
