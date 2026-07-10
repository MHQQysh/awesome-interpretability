# 282. Quantifying Feature Space Universality Across Large Language Models via Sparse Autoencoders

- **Authors:** Michael Lan, Philip Torr, Austin Meek, Ashkan Khakzar, David Krueger, Fazl Barez
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2410.06981
- **Tags:** Feature Universality, SAE, SVCCA, RSA, Cross-Model Alignment

## Introduction

Universality hypothesis 认为不同 LLM 可能学习相似概念表示，但 neuron polysemanticity 让“同一 feature 是否对应”很难直接比较。论文提出 weaker 的 Analogous Feature Universality：即使不同模型的 SAE feature 不一一相同，它们张成的 semantic subspaces 和 feature relations 是否相似。

## Method / Framework

作者在不同模型/层训练或使用 SAE，先按同一 token 上的 activation correlation 配对跨模型 feature；再用 SVCCA 比较 feature subspace 的全局对齐，用 RSA 比较 feature relation/距离矩阵。为避免“任意同维 subspace 都相似”，构造 paired-feature shuffle 与 random feature-subset null tests，分别检查 feature pairing 必要性和 semantic subspace size 是否足够解释相似度。

## Baselines / Comparisons

实验比较 Pythia-70M/160M、Gemma-1/2-2B 等同 tokenizer 模型，分析不同层、不同概念类别（time、calendar、nature、countries、people/roles、emotions）。baseline 是 random shuffling、random feature subsets 和非匹配层/模型；指标为 SVCCA、RSA、p-value 和 feature activation correlation。

## Experiments / Findings

Pythia-70M L3 与 Pythia-160M L5 的 semantic subspace 示例中，Time SVCCA 0.59、Calendar 0.65、Nature 0.50、Countries 0.72、People/Roles 0.50、Emotions 0.83，而随机均值仅约 0.05-0.15，p-value 多为 0。中层匹配通常更相似，支持“弱普适 feature/subspace”；但 Country 等类别在 Gemma 跨版本上不总通过检验，且有相当比例 SAE-specific/idiosyncratic features。

## Ablation / Error Analysis

只比较 SVCCA 会把任意高维子空间相似误判为 universality，因此 paired shuffle 和 random-subspace test 是必要消融。不同 tokenizer、SAE dictionary width、layer pair、feature annotation keywords 和 activation sampling 都会影响结果；高相关 pairing 也可能被共同 token frequency 驱动，而非真正语义同构。

## Limitations

SVCCA/RSA 只能证明几何相似，不证明两个模型使用同一算法或能互换 circuit；SAE 自身的 feature splitting/occlusion 会制造或掩盖 universality。研究覆盖模型规模和概念有限，开放域、安全相关 feature 的普适性还未确定。

## 两句话总结

论文用 activation pairing、SVCCA 和 RSA 把跨 LLM 的 SAE feature universality 从“一一对应”放宽到“子空间和关系相似”，并发现中层语义子空间存在弱普适性。它同时发现不少 SAE-specific feature，说明跨模型迁移 interpretability 需要对齐测试和 null baseline，而不能只看相似图。

## Evidence note

本笔记逐段核对 arXiv PDF 的 universality definitions、pairing/null tests、Tables 1-2、跨模型实验与 Conclusion。

