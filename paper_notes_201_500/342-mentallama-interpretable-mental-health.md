# 342. MentaLLaMA: Interpretable Mental Health Analysis on Social Media with Large Language Models

- **Authors:** Kailai Yang, Tianlin Zhang, Ziyan Kuang, Qianqian Xie, Jimin Huang, Sophia Ananiadou
- **Venue / year:** WWW 2024
- **Paper:** https://arxiv.org/abs/2309.13567
- **Tags:** `mental-health` `instruction-tuning` `social-media` `explanations` `MentaLLaMA`

## Introduction
Social-media mental-health systems traditionally use discriminative classifiers for depression, stress, loneliness, suicide risk, and related tasks. They can be accurate but usually output only a label, which is inadequate when a user or clinician needs evidence for the decision. General LLMs can produce explanations, but zero-shot/few-shot classification is weak and the explanations become unreliable when the prediction is wrong.

The paper addresses two practical bottlenecks: there is little open instruction data containing both mental-health labels and detailed rationales, and there was no released open-source LLM specifically tuned for interpretable mental-health analysis. The authors build a multi-source instruction corpus and train MentaLLaMA models that output a prediction plus evidence-oriented explanation.

## Method / Framework
The Interpretable Mental Health Instruction (IMHI) dataset combines 10 sources covering eight tasks, including binary and multi-class mental-health detection, cause detection, wellness dimensions, loneliness, and interpersonal risk factors. The raw social-media examples come from Reddit, Twitter, SMS, and related sources. The reported instruction corpus contains about 105K samples; the merged IMHI training set used for instruction tuning contains 72,095 samples after processing.

For each annotated post, expert-designed few-shot prompts tell ChatGPT the task, label, and desired evidence format. ChatGPT generates an explanation. The authors automatically check correctness and consistency on all collected data, use BARTScore against expert-written gold explanations, and perform human evaluation on sampled outputs. Explanations are retained only when the label and evidence are sufficiently aligned. Rule-based templates convert each task into query-answer instruction pairs, and an IMHI-completion variant is created for models with weaker instruction following.

MentaLLaMA is trained from Llama-2 base and chat checkpoints. The released variants include MentaLLaMA-7B, MentaLLaMA-chat-7B, and MentaLLaMA-chat-13B. The instruction-tuned model generates the label and a natural-language explanation in one response. The benchmark has 10 test sets, with weighted F1 for correctness and BARTScore/human ratings for explanation quality.

## Baselines / Comparisons
Correctness baselines include task-specific discriminative models such as MentalBERT/MentalRoBERTa, T5, BART, and other supervised PLMs. Generative/LLM baselines include zero-shot and few-shot ChatGPT, GPT-4, Llama-2 chat, and completion-trained models. The comparison is intentionally broad: domain-specific discriminative models remain strong on pure classification, while MentaLLaMA is judged on the joint label-plus-explanation objective.

The paper also evaluates generalization by withholding Dreaddit, T-SID, IRF, and related task data during training, then testing on those unseen tasks. This is more informative than only reporting in-domain performance because instruction tuning could otherwise memorize task-specific label patterns.

## Experiments / Findings
MentaLLaMA-chat-13B surpasses or approaches state-of-the-art discriminative methods on 7 of 10 test sets in correctness. In the detailed table, it reaches strong weighted F1 on CAMS, IRF, loneliness, MultiWD, SAD, SWMH, and T-SID, although MentalBERT remains better on some established single-task datasets. Zero-shot ChatGPT is substantially weaker on several tasks; few-shot prompting improves it but remains below the tuned open model on many datasets.

Explanation quality is the complementary result. MentaLLaMA-chat-7B improves over Llama-2 chat and is broadly comparable to ChatGPT on consistency, evidence support, professionality, and overall quality. The 13B chat model is closer to human preferences and consistently better than the base generative baselines, although ChatGPT can retain an advantage in domain knowledge. This is the expected trade-off: a smaller open model can learn the output format and task behavior, but it does not automatically possess ChatGPT's broader mental-health knowledge.

The generalization experiment shows that MentaLLaMA-chat models outperform zero-shot ChatGPT on several unseen datasets and outperform T5/BART on some held-out tasks. The result supports the value of multi-task, multi-source instruction tuning rather than training one classifier per condition. Human evaluation by three psychology Ph.D. students scores consistency, evidence trustworthiness, professionality, and overall usefulness; the MentaLLaMA-13B explanations are reported as comparable to ChatGPT on most dimensions.

## Ablation / Error Analysis
The key data ablation is IMHI versus IMHI-completion. The instruction form is more natural for chat models; completion formatting can reduce performance when the base model has poor instruction following. The model-size comparison shows that 13B improves explanation quality and held-out correctness, but 7B is already useful and much cheaper.

The paper also separates prediction correctness from explanation quality. A fluent rationale generated for a wrong label is not accepted as a correct explanation. Automatic consistency checks use label/explanation pairs and a classifier trained on the generated evidence; the authors report high consistency on the gold explanation set. Human analysis still finds that MentaLLaMA lacks some domain-specific knowledge and can produce unsupported or incomplete evidence, especially for subtle psychological causes.

## Limitations
Mental-health social-media data are sensitive, noisy, culturally dependent, and subject to privacy/consent concerns. ChatGPT-generated rationales are silver labels, not clinical explanations, and filtering can introduce bias. Weighted F1 and BARTScore do not prove clinical validity or causal correctness. The benchmark covers English social posts and a limited set of mental-health tasks; deployment would require domain experts, calibrated uncertainty, privacy protection, and explicit safeguards against over-interpreting a model response.

## 两句话总结
MentaLLaMA 用 105K 规模的多来源 IMHI 指令数据，把“心理健康分类”和“给出证据解释”联合训练成开源 Llama-2 系列模型。它在 10 个测试集上接近或超过多数判别式基线并能生成接近 ChatGPT 的解释，但数据来自敏感社交媒体且 rationale 多为 ChatGPT 生成，不能直接当作临床因果结论。

## Evidence note
This note was written from the WWW 2024 paper PDF, including IMHI construction, automatic/human explanation evaluation, Tables 1-3, MentaLLaMA variants, unseen-task generalization, and limitations.
