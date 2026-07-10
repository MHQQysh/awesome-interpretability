# 363. Not All Language Model Features Are One-Dimensionally Linear

- **Authors:** Joshua Engels, Eric J. Michaud, Isaac Liao, Wes Gurnee, Max Tegmark
- **Venue / year:** ICLR 2025
- **Paper:** https://arxiv.org/abs/2405.14860
- **Tags:** `mechanistic-interpretability` `sparse-autoencoder` `multidimensional-feature` `causal-intervention`

## Introduction
The linear representation hypothesis has encouraged mechanistic interpretability to search for one-dimensional directions representing concepts. This paper asks whether some concepts are intrinsically multi-dimensional and cannot be decomposed into independent or non-co-occurring one-dimensional features.

The answer is yes. The authors find interpretable circles for days of the week and months of the year in GPT-2 and Mistral 7B, then show that Mistral 7B and Llama 3 8B use those circles for modular arithmetic. This matters because treating every SAE feature as a scalar can hide the variables and algorithms actually used by the model.

## Method / Framework
The paper defines a feature as a vector-valued function and defines reducibility through an affine rotation/translation into separable or mixture distributions. An irreducible feature is one for which no such decomposition exists. The practical search uses sparse autoencoders, clustering of dictionary elements, PCA, and a soft separability/reducibility measure.

The authors first discover candidate planes in GPT-2 and Mistral 7B, then test causal use. A circular probe maps a five-dimensional PCA subspace to an angle on a weekday heptagon or month dodecagon. Interventions replace the model's projected state with a clean point on the circle and average-ablate the remaining dimensions. Baselines patch the first five PCA dimensions, patch the entire layer, or replace the layer with its average.

## Baselines / Comparisons
The conceptual comparison is with one-dimensional SAE features and the two-part linear representation hypothesis. Related mechanistic baselines include toy modular-arithmetic models, GPT-2 position helices, linear relation probes, and non-causal U-shaped numeric representations.

For interventions, the paper compares the circular probe with whole-layer patching, clean-run PCA patching, per-layer probes, and an SAE-derived probe evaluated across neighboring layers. These baselines test whether the discovered plane is merely correlated with the task or is sufficient to drive the correct modular output.

## Experiments / Findings
The SAE search recovers circles for weekdays, months, and some years. The weekday/month representations are causally implicated in solving modular addition: across models and tasks, intervening on the circular subspace produces nearly the same effect as patching the whole layer and usually beats patching only the top PCA dimensions.

Off-distribution interventions over radius and angle show that the model treats angle as the encoded quantity rather than merely memorizing seven or twelve points. The Mistral layer-8 SAE probe remains effective when shifted to nearby layers; on layer 6, the layer-8 circular probe has average logit difference 0.029, while the SAE probe reaches -2.32. Synthetic “very early/very late” and morning/evening prompts also map to intermediate positions on the circle, suggesting continuity of time.

## Ablation / Error Analysis
The central ablation is the intervention comparison. Patching the entire layer gives an upper bound, while patching only the circular subspace tests sufficiency and average ablating the rest prevents backup circuits from confounding the effect. The SAE-plane comparison shows that a discovered plane is slightly weaker than a manually fitted per-layer probe on its original layer but more robust across layer shifts.

The authors do not find many equally interpretable multi-dimensional features. A high score may be missed by the clustering method, may live in a dimension higher than two, or may reflect a feature that humans have not interpreted. Their reducibility statistics are softened approximations and are statistical rather than intervention-based definitions.

## Limitations
Only a subset of models and behaviors is studied, and the search may be biased toward two-dimensional circles that are easy to visualize. The irreducibility definition is not itself causal, the practical reducibility index can be subjective, and the work does not yet show that all important computation uses multi-dimensional features. Higher-dimensional manifolds and more robust discovery methods remain open.

## 两句话总结
这篇 ICLR 论文证明大模型里存在不能拆成独立一维方向的多维特征，并在 GPT-2/Mistral 中找到表示星期和月份的圆形子空间。通过干预圆的角度，作者显示这些表示是模块化算术的真正计算变量，而不是只与答案相关的可视化巧合。

## Evidence note
本笔记依据 ICLR 2025/arXiv v3 全文逐节核对；干预实验覆盖 49 个 weekday prompts 和 144 个 month prompts，数字和比较按正文记录。
