# 334. Explainable and Interpretable Multimodal Large Language Models: A Comprehensive Survey

- **Authors:** Yunkai Dang, Kaichen Huang, Jiahao Huo, Yibo Yan, Sirui Huang, Dongrui Liu, Mengxi Gao, Jie Zhang, Chen Qian, Kun Wang, Yong Liu, Jing Shao, Hui Xiong, Xuming Hu
- **Venue / year:** arXiv 2024 (arXiv:2412.02104)
- **Paper:** https://arxiv.org/abs/2412.02104
- **Tags:** `survey` `multimodal-llm` `explainability` `interpretability` `vision-language`

## Introduction
MLLMs combine visual, textual, audio, and video signals, but their cross-modal projection and large Transformer stacks make it difficult to tell why a prediction was made. The survey separates two notions: interpretability concerns understanding the model's internal transformation from inputs to outputs, while explainability concerns external, often post-hoc explanations that communicate the decision to a user. This distinction prevents a saliency map or a fluent rationale from being treated as evidence that the internal mechanism is understood.

The paper's main contribution is organizational rather than a new model. It classifies the literature along three perspectives: Data, Model, and Training & Inference. Within those perspectives it further distinguishes token-, embedding-, neuron-, layer-, and architecture-level analysis, as well as input/output explanations, benchmarks, and domain applications. The goal is to make a fragmented MLLM literature searchable by the object being explained and by the stage at which transparency is introduced.

## Method / Framework
The survey framework has three top-level branches. **Data** covers input/output explainability, benchmarks, and applications. Input/output methods include perturbation, saliency, attribution, contrastive/counterfactual analysis, and causal inference across modalities. Benchmark work evaluates alignment with human attention, robustness under distribution shift, truthfulness, safety, fairness, privacy, and multimodal uncertainty. Application work includes science, medicine, video, autonomous driving, audio, and other domain-specific systems.

**Model** covers internal units and architecture. Token-level work analyzes visual tokens and visual-text tokens. Embedding-level work studies visual, textual, and cross-modal representations using probing, sparse/interpretable vector decompositions, shared concept spaces, and graph structures. Neuron-level work identifies individual knowledge/concept neurons or groups of specialized units. Layer-level work traces where information is formed, copied, merged, or transformed. Architecture-level analysis studies attribution, workflow, attention, and causal information flow; architecture design adds interpretable bottlenecks, modules, or human-readable intermediate structures.

**Training & Inference** collects methods that alter training objectives, adapters, alignment, data selection, or inference-time behavior. Training can encourage interpretable representations, reduce hallucination, align visual and textual features, or learn concept bottlenecks. Inference methods can use visual retracing, attention control, counterfactual prompts, uncertainty estimates, or explanation agents without retraining the base MLLM.

## Baselines / Comparisons
As a survey, there is no single numerical baseline. The paper compares families of techniques by explanation type, model component, modality, task, control signal, and whether a method analyzes an existing model or designs a more interpretable architecture. It positions token/embedding attribution against neuron and layer analysis, and post-hoc heatmaps against causal/architectural methods.

The survey also contrasts single-modality XAI with genuinely multimodal explanations. CAM, Grad-CAM, LIME, SHAP, and text-only attribution are useful references, but they do not automatically explain the interaction between a visual token and a language token. The included tables organize representative MLLM papers rather than reporting a common leaderboard, so “comparison” should be read as taxonomy and coverage comparison, not as a controlled experiment.

## Experiments / Findings
The main finding is that MLLM interpretability is moving from output-only visualization toward cross-modal internal analysis. Data-level methods expose which region, word, frame, or modality affected an output; embedding-level methods try to map dense representations to concepts; neuron/layer methods explain where information is stored and propagated; and architecture-level methods explain how the vision encoder, projector, language model, and cross-attention workflow cooperate.

The survey highlights causal inference as an important bridge. Connectivity-contrastive learning, causal representation learning, attribution, and activation intervention can distinguish correlation from a component that actually changes the output. It also highlights sparse/interpretable embedding methods because multimodal representations are often polysemantic: a human-readable sparse basis makes it possible to inspect concepts without assuming that a raw neuron has one stable meaning.

On evaluation, the survey repeatedly identifies a mismatch between plausible explanations and faithful explanations. Human attention alignment, saliency quality, textual overlap, and LLM-as-a-judge scores measure different things. Benchmarks such as VISTA, BenchLMM, MultiTrust, MUB, and domain-specific datasets are presented as steps toward measuring alignment, robustness, safety, fairness, privacy, and uncertainty together rather than reporting only task accuracy.

The application survey shows that interpretability requirements depend on modality and domain. Medical and autonomous-driving systems need traceable evidence and uncertainty; video and audio systems need temporal attribution; document and science systems need fine-grained cross-modal grounding. A single universal heatmap cannot satisfy all these use cases.

## Ablation / Error Analysis
There is no conventional ablation because this is a literature survey. The useful “negative result” is the taxonomy's diagnosis of evaluation failure: an explanation can be fluent but not causally connected, visually localized but semantically incomplete, or aligned with human attention but not with the model's decision. The survey therefore treats input perturbation, causal intervention, robustness checks, and human evaluation as complementary rather than interchangeable.

The paper also makes an implicit scope ablation by excluding purely unimodal explainability and generic Transformer interpretability unless they address multimodal interaction. This keeps the framework focused, but it means some mechanistic tools developed for text-only LLMs appear only as methodological foundations rather than as fully evaluated MLLM methods.

## Limitations
The taxonomy is broad and the included methods use incompatible datasets, models, explanation formats, and metrics, so the survey cannot establish a universal ranking. Many cited methods still rely on post-hoc proxies, weak automatic judges, or small human studies. The field lacks standardized causal faithfulness tests for cross-modal explanations, and the survey itself points to the difficulty of comparing a visual mask, a textual rationale, and an internal feature circuit on one scale.

## 两句话总结
这篇综述把 MLLM 可解释性整理成 Data、Model、Training & Inference 三个视角，并进一步覆盖 token、embedding、neuron、layer 与 architecture 层级。它最重要的判断是跨模态解释不能停留在漂亮的 heatmap 或流畅文本，而需要用因果干预、跨模态 grounding、鲁棒性和人类评价共同验证。

## Evidence note
This note was written from the full survey PDF (arXiv:2412.02104v1), including Figure 2's taxonomy, the token/embedding/neuron/layer/architecture sections, benchmark/application discussion, future directions, and conclusion.
