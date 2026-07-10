# 336. Interpreting Bias in Large Language Models: A Feature-Based Approach

- **Authors:** Nirmalendu Prakash, Roy Ka-Wei Lee
- **Venue / year:** arXiv 2024 (arXiv:2406.12347)
- **Paper:** https://arxiv.org/abs/2406.12347
- **Tags:** `bias` `mechanistic-interpretability` `activation-patching` `attribution-patching` `debiasing`

## Introduction
LLMs reproduce social associations present in their training data, but a dataset-level bias score does not explain where the association enters the computation. Earlier causal mediation work often localized bias to sparse attention heads in GPT-2. This paper asks a more granular question for larger decoder-only models: can a human-defined feature map track how an association is formed, copied, combined, and finally expressed as a biased token?

The motivating examples are “The doctor said that” -> “he” and “Washing the dishes is the duty of the” -> “wife”. The authors distinguish an early feature origin from later components that copy or amplify it. This matters for mitigation: editing the component that creates an association should have a different effect from editing a component that only transports it.

## Method / Framework
The paper defines a feature map as a hypothesis about latent concepts such as male/female, household, or grammatical structure. It tests the hypothesis with activation patching and attribution patching. A counterfactual prompt swaps a stereotypical profession or noun with a contrasting one, and the residual stream, MLP outputs, or attention-head outputs are patched from one run into the other. The causal readout is the logit difference/order for stereotypical versus anti-stereotypical pronouns.

The experimental models are LLaMA-2-7B, LLaMA-3-8B, and Mistral-7B-v0.3. The Professions dataset supplies the main templates; WinoBias and CrowSPairs evaluate the effect after debiasing. The Pandas counterfactual corpus supplies 18,380 gender-swapped sentences for targeted fine-tuning. The bias score is the fraction of samples where the model prefers the anti-stereotypical sentence over its stereotypical counterpart.

The analysis proceeds in stages. First, lower-layer MLP outputs at the profession token are patched to test the origin of the gender feature. Next, attribution patching identifies attention heads that copy the profession information to later positions; the corresponding heads are activation-patched together because scores are distributed across many heads in a layer. Finally, selected MLPs or attention heads are fine-tuned on counterfactual data to test targeted debiasing.

## Baselines / Comparisons
The paper is primarily a mechanistic comparison rather than a leaderboard. It compares lower-layer MLP intervention, copying-head intervention, and upper-layer MLP intervention. It also compares MLP-based debiasing with attention-head debiasing, while evaluating the untouched model on Professions, WinoBias, and CrowSPairs.

The conceptual baselines are prior causal mediation and circuit methods that localize bias to neurons or heads without first specifying a feature evolution hypothesis. The authors' feature-based approach is intended to be case-specific: a component responsible for one gender association may not be responsible for another.

## Experiments / Findings
For the profession templates, patching lower-layer MLP activations often reverses the stereotypical logit order. The effect is strongest in early layers: the paper reports reversal for about 75% of LLaMA-3-8B samples and 73% of Mistral-7B samples in the main early-layer experiment, with LLaMA-2-7B showing fewer reversals. By layers around 20-22, more than 70% of samples already exhibit a biased logit order, indicating that the association is formed early and then propagated/amplified.

Attribution scores for individual attention heads are often diffuse, so patching only the top few heads produces no reversal. When the authors collect the relevant layers and patch a larger set, k=90 gives reversal for about 80% of LLaMA-3-8B samples and about 70% of LLaMA-2-7B samples; extending the layer range yields about 85% and 83% for LLaMA-2-7B and Mistral-7B. This supports the “copying heads” hypothesis, but also shows that the mechanism is distributed rather than a single magic head.

Upper-layer MLP patching has a much smaller effect: logit order remains unchanged in 93% of LLaMA-3-8B cases, no reversal is observed for LLaMA-2-7B, and only 3% reverse for Mistral-7B. The model-specific exception cases associate verbs and professions, suggesting that later MLPs can amplify or combine gender information even when they are not its origin.

The second feature-map example finds that the household and woman features for “dishes” are derived by an early MLP, while the final “wife” token depends on combining those features with grammatical structure in later layers. This is a concrete example of a stereotyped output being assembled rather than stored as a single neuron.

## Ablation / Error Analysis
Targeted debiasing on layers 1-4 MLPs and on the top copying heads reduces biased outputs, but the effect depends strongly on the model. MLP debiasing is largely ineffective on LLaMA-3-8B and LLaMA-2-7B and more effective on Mistral-7B. Attention-head debiasing reduces WinoBias/CrowSPairs scores for LLaMA-3-8B more visibly, while the table shows different trade-offs across models; the profession task is often retained better than the external bias sets.

The generation check is important: after early MLP patching, the profession term is usually retained and pronouns switch as expected. Thus the intervention is not simply erasing the sentence context. Ambiguous samples remain, and compensation between heads can hide a causal effect when one component is patched.

## Limitations
The study uses English and narrow binary-gender templates, so the feature maps may need redesign for racial, cultural, multilingual, or intersectional bias. A 0.5 preference score is not proof of a bias-free model; it is only a relative comparison. Activation patching can be confounded by compensatory pathways, and small counterfactual fine-tuning can alter language behavior. The proposed case-by-case workflow is interpretable but expensive to repeat for many bias types.

## 两句话总结
这篇文章把偏见当作在模型中逐层演化的 feature map，用 activation/attribution patching 区分 bias origin、copying heads 和后层组合，而不是只报告一个总体偏差分数。实验显示早层 MLP 和分布式 attention copying heads 是关键干预点，但不同模型的偏见路径不同，因此 targeted debiasing 不能简单复用同一组组件。

## Evidence note
This note was written from the paper PDF (arXiv:2406.12347v1), including the hypotheses in Section 4, reversal rates, Tables 2-5, the “dishes/wife” case study, and the limitations section.
