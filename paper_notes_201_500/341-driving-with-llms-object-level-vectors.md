# 341. Driving with LLMs: Fusing Object-Level Vector Modality for Explainable Autonomous Driving

- **Authors:** Long Chen, Oleg Sinavski, Jan Hünermann, Alice Karnsund, Andrew James Willmott, Danny Birch, Daniel Maund, Jamie Shotton
- **Venue / year:** ICRA 2024
- **Paper:** https://arxiv.org/abs/2310.01957
- **Tags:** `autonomous-driving` `multimodal-llm` `vector-modality` `explainable-control`

## Introduction
Most autonomous-driving policies consume images or structured vectors, but their outputs are difficult for a human to question. A language model can express reasons and answer open-ended questions, yet a text-only LLM does not naturally understand object positions, speeds, traffic lights, or route geometry. This paper asks whether object-level numeric vectors can be grounded into a pretrained LLM without first converting the whole scene to pixels.

The work is deliberately a preliminary baseline. It targets open-loop simulated driving, perception questions, action prediction, and natural-language explanations. The authors also investigate whether LLM pretraining contributes useful driving priors, while acknowledging that the comparison with a conventional policy is not perfectly matched because the LLM receives much more question-answer supervision.

## Method / Framework
The data come from a procedurally generated 2D simulator. An RL expert trained with PPO observes object-level ground truth and supplies control actions. The authors collect 100K scenarios for vector-representation pretraining, 10K scenarios for QA labeling/fine-tuning, and 1K held-out evaluation scenarios across 15 virtual environments.

The `lanGen` function converts vectors into structured pseudo-language: object type, angle, distance, speed, traffic-light state, route, and optionally the expert's attention and action labels. GPT-3.5 is prompted as a driving instructor to generate 16 QA pairs per scenario. Guardrails ask the model to refuse claims about objects not present in the observation and to separate out-of-scope questions from driving questions. The resulting set contains about 160K QA pairs derived from 10K scenarios.

The LLM-Driver has a Vector Encoder, a Vector Former, and a frozen Llama-7B with LoRA. The Vector Encoder maps car, pedestrian, ego, and route vectors into latent tokens; cross-attention injects ego state; the Vector Former uses self/cross-attention with question tokens to create embeddings the LLM can decode. Stage 1 freezes the LLM and learns to caption vector representations using 100K QA pairs plus randomly sampled vectors. Stage 2 trains the vector modules and LoRA on driving QA and action-triggering questions. Actions are extracted with regular expressions so the language output can control the simulator.

## Baselines / Comparisons
The main baseline is Perceiver-BC, which uses the same Vector Encoder and Vector Former but replaces the pretrained LLM/LoRA with a Transformer policy. It has about 25M trainable parameters and is trained on perception and action outputs, not the 16 QA pairs per scenario. The paper also compares LLM-Driver with and without vector-representation pretraining, and includes constant “I don't know,” shuffled-answer, and GPT-generated answers for the open-ended QA evaluation.

## Experiments / Findings
On the 1K evaluation scenarios, pretraining substantially improves perception and action metrics. With pretraining, LLM-Driver reduces car-count MAE from 0.101 to 0.066, pedestrian-count MAE from 1.668 to 0.313, and token loss from 0.644 to 0.502 relative to the no-pretraining variant. For normalized longitudinal action MAE it improves from 0.094 to 0.066. Perceiver-BC is better on simpler regression such as traffic-light distance, but the LLM policy is stronger on the reasoning-heavy action prediction task under the paper's training setup.

For Driving QA, GPT-3.5 grades each answer from 0-10 using the structured observation, question, and answer; 230 samples are also hand-scored. The no-pretraining LLM-Driver scores 7.48 by GPT and 6.63 by humans, while the pretrained model reaches 8.39 and 7.71. GPT-generated reference answers score 9.47/9.24, and constant or shuffled answers score only 2.92/0.0 and 3.88/0.26. Pretraining therefore improves the QA grade by about 9.1% GPT-side and 10.8% human-side.

Qualitatively, the model can answer what it sees, predict a brake/steering action, and state a reason such as slowing for pedestrians or stopping at a red light. The language interface makes the vector policy inspectable in a way that a regression-only policy is not.

## Ablation / Error Analysis
The two-stage pretraining ablation is the central result. Without it, the Vector Former has to learn both numeric grounding and reasoning from the smaller QA set, leading to poor object counts and a higher token loss. The Perceiver-BC comparison shows the trade-off: language-model priors help complex action reasoning, but the LLM is not automatically the best numeric regressor.

The evaluation also checks weak answers and uses human scoring because GPT judges can be lenient toward semantically close but incorrect responses. The authors report numeric inaccuracies and failures in direct action commands; these errors can produce an explanation that sounds reasonable while being inconsistent with the actual vector state.

## Limitations
The study is open-loop and simulated, so success does not establish closed-loop safety. The Perceiver-BC comparison is not fully fair because LLM-Driver sees many more QA pairs and uses language-model pretraining. GPT-generated QA labels can contain hallucinations despite guardrails. Inference is slow, exact numerical commands are brittle, and the model has not been tested on real-world sensor noise or rare safety-critical scenarios.

## 两句话总结
这篇文章把 car/pedestrian/route 等 object-level numeric vectors 通过 Vector Encoder、V-former 和 LoRA 接入 Llama-7B，让同一个模型既能做驾驶动作又能用语言解释。两阶段 grounding 预训练明显改善了感知和 Driving QA，但 Perceiver-BC 对简单数值回归更强，且当前结果仍受模拟环境、GPT 标注和 open-loop 评估限制。

## Evidence note
This note was written from the full paper PDF (arXiv:2310.01957v2), including data collection, lanGen, the two-stage architecture, Table 1 perception/action metrics, Table 2 GPT/human QA grades, and limitations.
