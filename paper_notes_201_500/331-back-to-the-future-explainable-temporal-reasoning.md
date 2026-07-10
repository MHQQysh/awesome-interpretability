# 331. Back to the Future: Towards Explainable Temporal Reasoning with Large Language Models

- **Authors:** Chenhan Yuan, Qianqian Xie, Jimin Huang, Sophia Ananiadou
- **Venue / year:** WWW 2024 (arXiv:2310.01074, 2023 preprint)
- **Paper:** https://arxiv.org/abs/2310.01074
- **Tags:** `temporal-reasoning` `event-forecasting` `instruction-tuning` `explainability` `TimeLlama`

## Introduction
The paper argues that most temporal-reasoning benchmarks stop at temporal-expression detection, normalization, relation extraction, or relatively short temporal knowledge-graph queries. Those tasks test whether a model can read an explicit past relation, but not whether it can combine several events, infer a future event at a future timestamp, and explain the chain that supports the prediction. The authors therefore define explainable temporal event forecasting as a joint task: given a document and temporal context, predict whether a future event occurs and provide a coherent multi-hop explanation.

The central research question is not simply whether a larger LLM can answer the task zero-shot. It is whether high-quality, structured supervision from temporal knowledge graphs can turn a generic LLM into a model that is simultaneously better at prediction and at human-readable justification. The paper also asks whether scale alone is sufficient, or whether data quality and instruction tuning matter more below 13B parameters.

## Method / Framework
The data contribution is ExpTime, a multi-source instruction-tuning and evaluation set with about 26K examples. It is built from ICEWS14, ICEWS18, and ICEWS0515 temporal knowledge-graph forecasting data. For each query, the authors extract a reasoning path from a temporal KG model or rule-based reasoning system, then convert the path into a document, answer, and explanation. The resulting examples contain positive, negative, and neutral cases rather than only positive future events; the reported training counts are 11,703 positive, 8,705 negative, and 5,360 neutral examples, with 435/300/266 test examples for the three categories.

The explanation generation pipeline is called Temporal Knowledge-Graph-Instructed Generation (GIG). A path is first rendered into a template explanation. An LLM is then asked to judge whether the explanation is correct, repair flawed reasoning when necessary, and polish the accepted explanation. This is more constrained than asking ChatGPT to invent a rationale from the document alone: the KG path supplies the latent proof structure, while the LLM supplies fluent language. Human annotation of a 1,200-example sample checks correctness, completeness, and fluency; the reported Cohen's kappa values are 0.73, 0.65, and 0.81 respectively.

Using ExpTime, the authors instruction-tune four Llama-2-based models: TimeLlama-7B, ChatTimeLlama-7B, TimeLlama-13B, and ChatTimeLlama-13B. The Chat variants start from dialogue-tuned Llama-2 checkpoints, while the non-Chat variants start from base checkpoints. The output format requires both the event-forecasting label and an explanation, so the same model is evaluated on classification and generation rather than on a separate explanation-only head.

## Baselines / Comparisons
The comparison set includes Flan-T5, BART, MPT-7B, Falcon-7B, Vicuna-7B, ChatGPT/GPT-3.5, Llama-2-7B-chat, and Llama-2-13B-chat. This mixes instruction-tuned encoder-decoder models, generic decoder-only models, dialogue-tuned open models, and a strong API model. The comparison is useful because it separates generic language fluency from temporal reasoning supervision: ChatGPT can write plausible prose, while the TimeLlama family has access to the task-specific temporal instruction data.

Prediction is measured with per-class and overall F1 on the gold temporal-reasoning test set. Explanation quality is evaluated with BLEU, ROUGE, and BERTScore against the human/golden explanations. The paper also reports the effect of instruction tuning and training-set size, and directly compares prompting ChatGPT to the GIG-generated supervision process.

## Experiments / Findings
The strongest result is the gap between generic LLM prompting and temporal instruction tuning. ChatGPT reaches an overall prediction F1 of 43.5, while TimeLlama-7B and ChatTimeLlama-7B reach 81.5 and 83.1; the 13B versions reach 87.3 and 88.4. On the explanation table, ChatTimeLlama-7B improves substantially over Llama-2-7B-chat, with the paper reporting gains of roughly 35.1 BLEU points, 19.3 ROUGE points, and 6.4 BERTScore points in the corresponding aggregate comparisons. The improvement is therefore not just a label-prediction effect: the tuned models also produce explanations closer to the reference explanations.

The class-wise results show that generic models are especially weak on negative and neutral temporal cases, whereas TimeLlama models remain high across all three labels. The best 13B Chat model is not merely winning because it is larger; the authors explicitly observe that a small amount of high-quality instruction data can produce large gains and that parameter count alone does not guarantee better temporal reasoning. In some comparisons, Llama-2-13B-chat is not better than the 7B checkpoint until the temporal supervision is added.

The data-construction experiment is also important. Directly prompting ChatGPT to produce explanations yields lower coherence and correctness than the GIG pipeline, even though ChatGPT is itself used inside GIG. The structured reasoning-path prompt gives the generator an explicit sequence of events to verbalize, reducing unsupported jumps and making the final explanation align with the answer label. This is an early example of using a symbolic or graph proof as an explanation scaffold for an LLM.

## Ablation / Error Analysis
The paper varies instruction-tuning size and compares the tuned models with their untuned counterparts. The qualitative trend is that high-quality examples are more valuable than simply increasing the number of weakly generated examples. The 7B models already gain large improvements, so the result is not explained by a pure scaling law.

The authors also inspect explanation quality separately from event prediction. A model can sometimes obtain a reasonable temporal label while producing a generic or incomplete rationale; conversely, a fluent explanation can be temporally wrong. This motivates reporting both F1 and text-similarity metrics rather than treating generated prose as evidence of reasoning. The annotation protocol's separate correctness, completeness, and fluency fields makes this distinction explicit.

## Limitations
ExpTime is derived from temporal KG datasets and therefore inherits their event coverage, timestamp distribution, relation vocabulary, and possible annotation artifacts. The explanations are grounded in extracted paths, so they test explanation of a supplied proof path more directly than open-ended discovery of all possible causes in a document. BLEU and ROUGE also reward lexical overlap and do not by themselves prove that a rationale is causally faithful. Finally, the paper's future-event setting is narrower than real news forecasting: real documents contain conflicting sources, missing timestamps, and events outside the KG ontology.

## 两句话总结
这篇文章把“未来事件预测”和“解释为什么预测它”合成了一个新的可评测任务，并用 temporal-KG reasoning path 约束 LLM 生成 ExpTime 数据。核心结论是高质量的结构化指令数据和 GIG 解释生成比单纯增大通用 LLM 更关键，TimeLlama 在预测 F1 和解释质量上都明显超过通用基线。

## Evidence note
This note was written from the paper PDF (arXiv:2310.01074v2), including the method, dataset statistics, baseline table, prediction/explanation tables, and the instruction-tuning discussion.
