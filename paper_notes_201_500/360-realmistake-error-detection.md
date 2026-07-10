# 360. Evaluating LLMs at Detecting Errors in LLM Responses

- **Authors:** Ryo Kamoi, Sarkar Snigdha Sarathi Das, Renze Lou, Jihyun Janice Ahn, Yilun Zhao, Xiaoxin Lu, Nan Zhang, Yusen Zhang, Ranran Haoran Zhang, Sujeeth Reddy Vummanthala, Salika Dave, Shaobo Qin, Arman Cohan, Wenpeng Yin, Rui Zhang
- **Venue / year:** COLM 2024
- **Paper:** https://arxiv.org/abs/2404.03602
- **Tags:** `llm-as-judge` `error-detection` `benchmark` `rationale-reliability`

## Introduction
As LLMs are used in real systems, a detector that can flag errors in their responses is more useful than a general preference judge. Existing benchmarks often contain subjective tasks, only one error type, or artificial tasks such as word sorting, making it difficult to assign objective binary labels and to know whether the detector works on realistic failures.

The paper introduces ReaLMistake, a benchmark of objective, realistic, and diverse mistakes in LLM responses. It asks three questions: how accurately do LLMs detect errors, are their explanations reliable, and can prompt changes or common evaluator tricks improve detection?

## Method / Framework
ReaLMistake contains 900 expert-annotated responses from GPT-4-0613 and Llama 2 70B. It covers three tasks: Math Word Problem Generation, Fine-grained Fact Verification, and Answerability Classification. The task construction deliberately makes strong models fail while keeping the input short enough for human checking.

The four error categories are reasoning correctness, instruction-following, context-faithfulness, and parameterized knowledge. Each item has a binary error/no-error label, categories, and a human explanation. The benchmark evaluates 12 LLM detectors with zero-shot prompts that ask for an explanation and a final binary decision, then tests wording bias, positional bias, self-consistency, multi-model majority vote, and human-written evaluation steps.

## Baselines / Comparisons
Prior benchmark baselines include MT-Bench/PandaLM/LLMEval-2 pairwise judgments, WikiBio GPT-3 generation, SummEdits, and BIG-Bench Mistake. These are compared by task subjectivity, number of examples, label type, and error coverage; ReaLMistake is the pointwise, binary, objective alternative.

The detector comparison includes Gemma 7B, Llama 2 13B/70B, Mistral 7B/Mixtral 8x7B, Qwen1.5 14B/72B, GPT-3.5, Gemini 1.0 Pro, Claude 3 Opus, and GPT-4 0613/0125. Random-label and expert-human performance are also included. Thus the paper tests both closed and open models and separates precision from recall instead of reporting only an overall score.

## Experiments / Findings
The central finding is low recall for strong LLMs: GPT-4 and Claude 3 can be precise but miss many errors, and all LLM detectors are substantially worse than humans. On the GPT-4 response set, for example, GPT-4-0125 has MathGen F1 62.1, Fine-grained Fact Verification 62.9, and Answerability Classification 62.1, while expert F1 is 90.0, 95.5, and 90.5 respectively. Llama 2 70B can have high precision but unstable recall across tasks.

The error detector's explanation is not reliable even when the binary label is correct. Closed models usually reason better than open models, but they still make mistakes, especially on MathGen; open-source detectors often provide wrong or incomplete reasoning for both erroneous and correct responses. Performance also correlates imperfectly with general model strength: stronger models tend to have higher precision but lower recall on GPT-4 errors.

## Ablation / Error Analysis
Prompt design is a strong ablation. Flipping the order of the two answer options increases recall by 16.0 +/- 21.7% for the error-sensitive prompt and 27.2 +/- 23.9% for the conservative prompt, showing positional bias. Changing “detect errors” to “evaluate whether valid,” while keeping the definition fixed, changes recall by 16.9 +/- 20.3% on average, showing wording bias.

The proposed improvements do not solve the problem. Five-sample self-consistency, majority voting across multiple LLMs, and human-written evaluation steps fail to improve error-detection performance consistently. The authors interpret this as evidence that error detection is not a superficial prompting problem; strong detectors need better careful reasoning and open models need much stronger reasoning reliability.

## Limitations
The benchmark intentionally focuses on objective errors, so results do not directly measure ambiguous or preference-dependent failures in summarization, dialogue, or open-ended writing. Human performance is evaluated on a small subset, and detector explanations are manually analyzed on sampled cases. The benchmark uses responses from two source models and three tasks, so generalization to other domains and models remains open.

## 两句话总结
ReaLMistake 用三个现实任务和四类客观错误构造了 900 条逐条可判定的 LLM 回复，并系统测试 12 个 LLM 作为错误检测器。结果显示强模型往往 precision 尚可但 recall 很低，理由也不可靠，且 self-consistency、投票和提示词微调都没有解决根本问题。

## Evidence note
本笔记依据 COLM 2024/arXiv v2 全文逐节核对；表 4 的模型级结果按论文原表和正文重点结论记录，避免把附录全部数字误写成主结论。
