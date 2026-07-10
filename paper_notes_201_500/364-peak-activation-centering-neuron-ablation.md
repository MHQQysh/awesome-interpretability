# 364. Investigating Neuron Ablation in Attention Heads: The Case for Peak Activation Centering

- **Authors:** Nicholas Pochinkov, Ben Pasero, Skylar Shibayama
- **Venue / year:** 2nd World Conference on Explainable Artificial Intelligence, 2024
- **Paper:** https://arxiv.org/abs/2408.17322
- **Tags:** `mechanistic-interpretability` `ablation` `activation` `pruning`

## Introduction
“Ablate and measure” is a standard mechanistic-interpretability tool, but setting an attention neuron to zero is not automatically neutral. The replacement value can both remove information and push the residual stream out of distribution. The paper studies this baseline choice and proposes **peak ablation**, which sets a neuron to its modal/default activation rather than blindly zeroing it.

The authors model a neuron as having a default mode that is perturbed when relevant features are present. If the default is nonzero or multimodal, zero ablation can inject more noise than a mode-centered intervention, which matters for causal scrubbing, pruning, and circuit analysis.

## Method / Framework
The study examines attention pre-output neurons in causal Mistral 7B and OPT 1.3B, masked RoBERTa Large, and ViT-Base/16. Text models are evaluated on 100,000 Pile tokens and ViT on 1,000 ImageNet-1k images using top-1 accuracy and cross-entropy.

Four replacement methods are compared: zero ablation, dataset mean ablation, activation resampling from random characters/tokens/generated text or pixels, and naive peak ablation. Neurons are randomly selected in 10% increments until the model is fully pruned, with deterministic selections across three seeds. Peak is estimated by histogram binning and sets activation to the center of the most populated bin.

## Baselines / Comparisons
Zero and mean ablation are the closed-form baselines; resampling is the causal-scrubbing-style stochastic baseline. The comparison spans decoder-only, masked-language, and vision Transformer regimes so that a method is not judged only on one activation distribution.

The paper reports performance at 50% and 90% pruning. This is not a claim that peak ablation is a better pruning algorithm in isolation; it tests which intervention least damages the model when the same neurons are removed.

## Experiments / Findings
For Mistral and OPT, peak ablation is the most consistently least damaging, followed by mean and zero, while random resampling causes the largest degradation. At 50% pruning, OPT top-1 is 44.54 peak, 42.39 mean, 41.77 zero, and 25.97-29.90 for the three resampling methods; Mistral is 51.30, 49.71, 48.03, and 17.93-27.01 respectively. At 90% pruning, peak remains slightly ahead of mean/zero for both decoder models.

For ViT, zero, mean, and peak are statistically similar and most loss depends on which neurons were selected rather than the replacement method. For RoBERTa, peak is slightly better through the first 75% pruning, but beyond that the curves become noisy and token-ID resampling can become the best method. The result is therefore model-dependent, not a universal dominance claim.

## Ablation / Error Analysis
The full method ablation is the comparison of four replacement values across pruning fractions and model families. The analysis separates information removal from distribution shift: zero-centered symmetric neurons make zero/mean reasonable, but atypical nonzero or multimodal neurons motivate peak centering.

Resampling is a particularly strong failure mode because random characters, token IDs, or generated text can produce activation values unlike the original input distribution. The paper also warns that the selected neuron subset itself can dominate the observed degradation, which is why methods must be compared at identical pruning masks.

## Limitations
Peak estimation depends on the profiling dataset and histogram bin size, and computing modal values for larger models may be expensive. The experiments target attention pre-output neurons, not all MLP activations, and the random-pruning setup does not establish the best structured sparse circuit. The paper offers a useful ablation protocol, not a full theory of activation distributions.

## 两句话总结
这篇论文指出神经元消融的替换值会同时影响“去掉了什么信息”和“把残差流推离分布多远”，因此提出把神经元置为最常见峰值的 peak ablation。它在 Mistral/OPT 上通常比 zero、mean 和随机重采样更少损伤性能，但 ViT 与 RoBERTa 的结果说明最佳中心化方式取决于模型和剪枝比例。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；表 2 的 50%/90% pruning 数值按原文记录，Resampling 的具体输入类型也按附录核对。
