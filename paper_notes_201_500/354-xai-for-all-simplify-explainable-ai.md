# 354. XAI for All: Can Large Language Models Simplify Explainable AI?

- **Authors:** Philip Mavrepis, Georgios Makridis, Georgios Fatouros, Vasileios Koukos, Maria Margarita Separdani, Dimosthenis Kyriazis
- **Venue / year:** arXiv preprint, 2024
- **Paper:** https://arxiv.org/abs/2401.13110
- **Tags:** `human-centered-xai` `gpt-builder` `audience-adaptation` `explanation-simplification`

## Introduction
Most XAI descriptions are written for developers and researchers, while practitioners often need a concise explanation of what an attribution plot means and what decision it supports. This paper asks whether a custom GPT can act as a presentation layer that translates XAI outputs into audience-specific, actionable language without requiring a new explainer for every underlying model.

The proposed system is called x-[plAIn]. It is intended for two audiences: non-technical end users who care about the insight and decision, and technical users who need more detail about the model, training, bias, or XAI method. The paper frames the contribution as human-centered XAI and method-agnostic explanation communication.

## Method / Framework
The authors build a custom GPT through GPT Builder rather than training a new foundation model. The prompt is developed in six versions, progressively adding clarity, relevance, actionability, user expertise, responsiveness, and a request for domain information. The final prompt asks for concise summaries, practical conclusions, and user-specific adaptation, and includes a simple “let's think step by step” instruction.

Five use cases supply the inputs: SHAP for boar-taint detection, LIME and Anchors for fake-news detection, an Integrated Gradients saliency-map verbalization for BERT, Grad-CAM for COVID-19 X-ray prediction, and PDP plots for hyperparameter optimization. The GPT receives the XAI visualization and, when necessary, the original problem definition, then produces a natural-language summary.

## Baselines / Comparisons
The direct baseline is the original description from the source XAI paper, shown alongside the x-[plAIn] description. The underlying XAI methods are also treated as technical reference points: LIME provides local surrogate feature influence, SHAP provides game-theoretic feature attribution, Grad-CAM highlights image regions, Integrated Gradients attributes input changes, and PDP summarizes hyperparameter effects.

The prompt versions form an internal progressive baseline from P1 to P6. This comparison shows what is gained by adding audience context, actionability, expertise questions, and response constraints. The paper does not compare multiple foundation models or train a task-specific explainer, so the results should be read as a usability proof of concept rather than a model leaderboard.

## Experiments / Findings
The evaluation is a questionnaire over the five use cases, comparing conventional technical descriptions with x-[plAIn] descriptions. More than 70% of participants reported comprehension of AI decision models below 60%, only about 30% actively used XAI methods, and over 80% preferred x-[plAIn] descriptions in the presented decision-making and image-understanding scenarios. Among participants with the highest self-reported understanding, x-[plAIn] was still preferred in 75% of cases.

The qualitative examples show that the GPT can correctly turn highlighted phrases or salient image regions into accessible explanations. Participants valued concise, relevant, and actionable language, and the tool was accepted by both end users and AI experts, though experts were more consistent in their preferences. The paper also reports that users wanted shorter outputs and stronger control over detail.

## Ablation / Error Analysis
The prompt-development table is the main ablation: P1 only requests clear summaries, while P6 adds expertise, domain, actionability, relevance, and a structured response policy. The paper also observes an important failure in the Integrated Gradients/SMV case: the GPT sometimes explains the image or underlying sentiment rather than the actual saliency map. It can add a true but unsupported observation, such as mentioning a strongly negative phrase not highlighted by the map.

This failure identifies the difference between contextual helpfulness and XAI faithfulness. A broadly sensible explanation can still be wrong about what the explainer showed. The authors therefore recommend active end users, shorter/longer modes, and more explicit context and user-profile controls.

## Limitations
The study is based on a small, questionnaire-style use-case evaluation and a custom GPT configuration, not a reproducible trained model. The prompts intentionally omit domain information to keep the survey level, which prevents a full demonstration of audience adaptation. GPT can explain the wrong artifact, verbosity remains difficult to control, and GDPR constraints prevent using user-specific data in the current setup.

## 两句话总结
这篇文章把 LLM 当作 XAI 的“翻译和呈现层”，用 GPT Builder 将 SHAP、LIME、Grad-CAM、Integrated Gradients 和 PDP 输出改写成面向不同知识水平的简洁建议。用户研究显示可读性和接受度明显改善，但 GPT 可能把图像或任务本身当成解释对象，说明解释简化不能替代对 XAI 输出的忠实约束。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；百分比来自论文的 questionnaire 结果，论文没有报告独立的多模型对照或大规模统计显著性检验。
