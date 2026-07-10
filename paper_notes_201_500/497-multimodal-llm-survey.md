# 497. A Survey on Multimodal Large Language Models

- **Authors:** Shukang Yin, Chaoyou Fu, Sirui Zhao, Ke Li, Xing Sun, Tong Xu, Enhong Chen
- **Venue / year:** IEEE TPAMI survey, arXiv version 2306.13549 (local text v4, 2024-11-29)
- **Paper:** [arXiv:2306.13549](https://arxiv.org/abs/2306.13549)
- **Tags:** `survey` `multimodal` `MLLM` `hallucination` `evaluation`

## Introduction
Multimodal large language models use a powerful language model as the reasoning and generation core while adding visual, audio, video, 3D, or other modalities. The survey's motivation is that instruction following, in-context learning, and Chain-of-Thought become qualitatively different when the model must ground language in an image or other signal. It therefore traces not only model architecture but also data, alignment, evaluation, hallucination, and multimodal reasoning.

## Method / Framework
The survey organizes MLLMs around three stages: architecture, training/data, and evaluation. Architectures connect an image encoder such as CLIP/EVA-CLIP to an LLM through a projection or query-based bridge, with variants for higher resolution, regions, video, audio, 3D, and multiple languages. Training is separated into pre-training for image-text alignment, instruction tuning for following multimodal instructions, and preference/alignment tuning such as RLHF or DPO.

The evaluation chapter distinguishes ordinary VQA/caption datasets from MLLM-specific benchmarks such as MME and MMBench, and from GPT-4 or human preference judgments. The survey then treats multimodal hallucination as a central interpretability/fidelity problem, dividing it into existence, attribute, and relationship hallucination. Later chapters cover multimodal ICL, multimodal CoT, LLM-aided visual reasoning, grounding, and specialized scenarios.

## Baselines / Comparisons
The survey compares frozen versus trainable visual encoders, dense versus token-efficient visual representations, linear projection versus Q-Former/query bridges, and pre-training/instruction-tuning/alignment paradigms. It catalogs open models including LLaVA, MiniGPT-4, InstructBLIP, BLIP-2, Flamingo, Qwen-VL, mPLUG-Owl, LLaMA-Adapter, and many later systems. For evaluation it contrasts accuracy/CIDEr-style task metrics, manually/GPT-judged open-ended answers, MME-style binary tests, and hallucination-specific benchmarks.

## Experiments / Findings
As a survey, it does not run one new model experiment. Its synthesis is that increasing language-model scale alone is insufficient: visual resolution, image-token budget, alignment data quality, instruction diversity, and the connector between vision and language all affect capability. Self-instructed multimodal data can improve instruction following, but the survey notes an important data tradeoff: diverse prompts improve breadth while high-quality fine-grained spatial annotations improve grounding and reduce hallucination.

The evaluation synthesis emphasizes that traditional VQA scores do not fully expose MLLM behavior. MME uses manually designed yes/no instructions to cover perception and cognition without directly reusing public annotations; MMBench and domain-specific benchmarks probe broader reasoning and instruction following. Open-ended judges provide richer feedback but introduce subjectivity, judge bias, and reproducibility concerns.

## Ablation / Error Analysis
The paper's error taxonomy is the main analysis. Existence hallucination invents an object, attribute hallucination gives a wrong property, and relationship hallucination invents a relation among objects. Proposed mitigations include negative/hallucination-aware data, preference tuning, visual contrastive decoding, multi-modal in-context examples, grounding/verification, and post-hoc correction such as Woodpecker; each moves the failure point rather than guaranteeing causal faithfulness.

## Limitations
The field evolves quickly and the local source is an updated survey version, so tables mix papers from different dates and evaluation protocols. Commercial MLLMs are not fully reproducible, multimodal judge scores can be noisy, and reported improvements may depend on resolution, prompt templates, or hidden data contamination. Survey coverage is comprehensive but is not a controlled meta-analysis of effect sizes.

## Two-sentence summary
This survey maps MLLM progress from visual-language connectors and multimodal training to evaluation, reasoning, grounding, and hallucination mitigation. Its central interpretability message is that a convincing multimodal answer must be checked against the visual evidence and the model's grounding behavior, not just language quality or a single VQA score.

## Evidence note
Read the full local arXiv v4 text, including architecture and encoder tables, training/data stages, evaluation section, multimodal hallucination taxonomy, mitigation methods, M-ICL/M-CoT/LAVR, and future directions.

