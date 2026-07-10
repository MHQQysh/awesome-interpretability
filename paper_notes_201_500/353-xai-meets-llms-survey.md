# 353. XAI meets LLMs: A Survey of the Relation between Explainable AI and Large Language Models

- **Authors:** Erik Cambria, Lorenzo Malandri, Fabio Mercorio, Navid Nobani, Andrea Seveso
- **Venue / year:** arXiv systematic mapping study, 2024
- **Paper:** https://arxiv.org/abs/2407.15248
- **Tags:** `xai-survey` `systematic-mapping` `llm-explainability` `rationales`

## Introduction
The survey asks how XAI techniques are integrated with LLMs, what trends are emerging, and which gaps remain. Its motivation is the combination of LLM opacity, hallucination, bias, misuse, privacy concerns, and the need for explanations that are both truthful and understandable to different stakeholders.

The paper makes an explicit distinction between **to explain** work, which tries to explain how an LLM works or make its opaque behavior more transparent, and **as feature** work, which uses LLM-generated explanations or rationales as input to another task. This distinction prevents every paper that prints a chain-of-thought from being counted as mechanistic interpretability.

## Method / Framework
The authors conduct a Systematic Mapping Study. They retrieve titles and abstracts from DBLP XML, Scopus/AMiner material, and arXiv computer-science papers, using OR keywords for XAI and LLM concepts and an AND condition between the two groups. From 1,030 initial manuscripts, manual false-positive screening produced 233 relevant papers, and the final study selected 35 papers using relevance and citation-per-year criteria.

The taxonomy has two macro-categories. Application papers are split into to-explain and as-feature studies; discussion papers are split into issues and benchmark/metrics. The paper then codes target model, whether a repository exists, model agnosticism, and goal categories such as comparison, explanation, improvement, interpretability, and reasoning.

## Baselines / Comparisons
The survey compares application patterns rather than models. To-explain examples include Inseq for attribution analysis, Boundless DAS for causal structure, saliency analysis of demonstrations, LMExplainer, and methods that translate feature importance into natural-language rationales. As-feature examples include using LLM explanations to train smaller language models, AMPLIFY, Language Guided Bottlenecks, and explanation-based reasoning improvements.

Discussion benchmarks include SCIENCEQA, ROSCOE, and prior LLM/XAI surveys. The paper contrasts local/global explanations, prompting/fine-tuning paradigms, and technical transparency with human-centered transparency. It also notes that many systems compare against black-box linear probes or conventional XAI methods, but do not necessarily explain the LLM itself.

## Experiments / Findings
The main quantitative result is the corpus reduction from 1,030 retrieved manuscripts to 35 selected studies, which demonstrates both the breadth of the keyword space and the small number of papers directly focused on the intersection. The qualitative synthesis finds that the literature is dominated by task-oriented use of LLMs, with interpretability often appearing as a byproduct rather than the primary objective.

The survey reports several recurring findings. LLM explanations can improve a downstream learner or recommendation interface, but their factual reliability is not guaranteed. CoT can improve accuracy while misrepresenting the actual reason for a prediction, and visual commonsense benchmarks show that even strong multimodal models struggle to explain unusual images. Open-source code release is increasing, but repository engagement is uneven.

The authors conclude that XAI methods must address both the model mechanism and the presentation layer. Explanations that are technically sophisticated but too difficult for non-technical stakeholders do not close the accountability gap, and future work should treat explainability as a first-class design requirement rather than an afterthought.

## Ablation / Error Analysis
There is no model ablation because this is a mapping study. Its error analysis is methodological: broad words such as “explain” and “interpret” yield false positives, abstracts can omit the actual explanation role, and citation-based selection may favor visible or older work over technically important new work.

At the paper level, the survey repeatedly flags post-hoc rationale failure, instability, hallucination, and the mismatch between a generated explanation and the causal evidence used by the model. This is why the taxonomy separates explanation generation from actual transparency and records whether methods are model-agnostic.

## Limitations
The final 35-paper sample is not exhaustive; it is selected after a large retrieval process and by citation-per-year relevance. The study includes preprints because the field changes quickly, so venue quality and publication status are heterogeneous. Its categories also rely on manual judgment, and one paper can legitimately belong to more than one category.

## 两句话总结
这篇系统映射研究把 LLM-XAI 文献分成“用 XAI 解释 LLM”和“把 LLM 解释当作下游特征”两大路线，并进一步区分应用、问题和评测工作。它发现直接解释 LLM 内部机制的研究仍然很少，未来需要同时提高解释的忠实性、面向用户的可读性和整个系统的责任可追踪性。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；论文自身是系统映射研究，不应把被综述论文中的实验数字当作本 survey 的新实验结果。
