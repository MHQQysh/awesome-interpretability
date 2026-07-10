# 338. FaithLM: Towards Faithful Explanations for Large Language Models

- **Authors:** Yu-Neng Chuang, Guanchu Wang, Chia-Yuan Chang, Ruixiang Tang, Shaochen Zhong, Fan Yang, Mengnan Du, Xuanting Cai, Vladimir Braverman, Xia Hu
- **Venue / year:** arXiv 2024 (version inspected: arXiv:2402.04678v4)
- **Paper:** https://arxiv.org/abs/2402.04678
- **Tags:** `faithfulness` `post-hoc-explanation` `contrary-hint` `prompt-optimization`

## Introduction
LLM-generated explanations are often plausible but may not describe the information that actually determined the answer. Chain-of-thought and self-refinement improve reasoning or answer quality, but neither guarantees that the displayed text is a faithful post-hoc explanation of the final decision. FaithLM focuses specifically on post-hoc explanation: the target LLM first answers, and a separate explainer must describe why that answer was produced.

The paper's central idea is to test an explanation semantically instead of masking its tokens. If an explanation says that a particular proposition matters, adding a contrary hint that asserts the opposite should change the target model's prediction. A large, directionally consistent prediction shift is treated as evidence that the explanation contains decision-relevant information.

## Method / Framework
For input X, target model f, answer Y, and explanation E, FaithLM asks an explainer LLM g to generate E and a contrary explanation not-E. The contrary hint is appended to the original question and the target model is queried again. The fidelity score is a divergence or prediction shift between the original output and the output under the contrary hint. The paper gives a latent-context intervention theorem: under contextual stability and a correct semantic contradiction, a zero shift indicates non-faithfulness while a positive shift indicates a faithful decision-relevant proposition.

FaithLM has two iterative optimization procedures. In fidelity-enhanced explanation, a human-crafted trigger prompt generates an explanation, an LLM agent constructs the contrary hint, and the target-model shift scores it. The explainer is then prompted with the trajectory of explanations and scores to produce a better one. In explanation-trigger-prompt optimization, the same fidelity signal scores candidate trigger prompts on a hold-out set of 30 training examples; the prompt is iteratively updated and transferred to new instances or related datasets.

The target models are Vicuna-7B and Phi-2. The explainers and contrary-hint agents are GPT-3.5-Turbo and Claude-2. Evaluation uses fidelity and truthfulness. Truthfulness is the semantic similarity to golden explanations, judged by GPT-4o and RoBERTa-Large/XLNet-Large NLI classifiers.

## Baselines / Comparisons
FaithLM is compared with SelfExp, which asks an LLM to generate a self-explanation in one forward pass, and Self-Consistency, which treats CoT outputs as explanations. These baselines represent the common practice of using a fluent rationale or multiple reasoning samples without an explicit intervention check.

The datasets are ECQA commonsense QA, TriviaQA-Long reading comprehension, and balanced COPA causal reasoning. ECQA and COPA use 500 examples in the reported API-budget setting; TriviaQA-Long uses 300 question-answer-evidence triples. FaithLM reports multiple explainer/target pairings and averages three runs.

## Experiments / Findings
Across all three datasets, FaithLM's fidelity curves rise with optimization steps and are higher than SelfExp and Self-Consistency after 20 steps. The result holds when Claude-2 or GPT-3.5 is used as the explainer and when Vicuna-7B or Phi-2 is the target. The gains are not only on the intervention metric: FaithLM explanations are more often judged similar to the ground-truth explanations by GPT and the NLI evaluators.

The trigger-prompt experiments show a generally ascending fidelity trajectory over 50 optimization rounds. Optimized prompts outperform the initial human-crafted prompt on ECQA, TriviaQA-Long, and COPA. More importantly, a prompt optimized on ECQA transfers to Social-IQA, and one optimized on COPA transfers to XCOPA without additional optimization, suggesting that the prompt captures a reusable explanation behavior rather than memorizing one dataset's wording.

The paper draws a careful distinction between explanation and reasoning. CoT can help a model reach an answer, but its steps may be generated for performance and need not be causal evidence for the final decision. FaithLM's contrary-hint intervention gives the explanation an operational test: if the stated reason is contradicted and the target model does not move, the sentence is likely post-hoc decoration.

## Ablation / Error Analysis
The contrary-hint ablation evaluates whether the generated opposite statement is actually contradictory rather than irrelevant. GPT-4o, RoBERTa-Large, and XLNet-Large classify original/contrary pairs as similar, dissimilar, or non-relevant. On sampled ECQA/COPA examples, the NLI classifiers reach up to about 86% and 82% in the dissimilar category. The ablation matters because a neutral hint would produce a low fidelity score even for a good explanation, while a poorly formed contradiction could create a spurious shift.

The optimization-step curves show diminishing returns and eventual convergence. Fewer steps reduce cost but often leave the explanation less faithful. The target/explainer swap is another robustness check: the method is not tied to a single open model, although the absolute fidelity depends on the LLM pair and evaluator.

## Limitations
Iterative generation and contrary-hint evaluation require many extra LLM calls, with corresponding energy, carbon, and monetary cost. The theorem depends on assumptions about semantic contradiction and contextual stability; a model can change because of prompt interference rather than because the explanation identified its real internal cause. Truthfulness is also judged by LLM/NLI systems and golden explanations, not by direct access to hidden computation. The current evaluation is text-only and future work is needed for high-stakes domains such as healthcare.

## 两句话总结
FaithLM 把解释忠实性定义成一种 intervention property：把解释改成 contrary hint 后，目标模型的预测应该发生可测的方向性变化。它用这个分数迭代优化 explanation 和 trigger prompt，在三个数据集上超过 SelfExp/CoT 类基线，但代价是更多 LLM 调用且仍依赖“反事实提示确实代表语义干预”的假设。

## Evidence note
This note was written from the inspected PDF version arXiv:2402.04678v4, including the contrary-hint theorem, Algorithms 1-2, datasets/baselines, Figures 3-7, the contrary-hint ablation, and the limitations section.
