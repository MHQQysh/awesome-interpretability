# 329. Universal Neurons in GPT2 Language Models

- **Authors:** Wes Gurnee, Theo Horsley, Zifan Carl Guo, Tara Rezaei Kheirkhah, Qinyi Sun, Will Hathaway, Neel Nanda, Dimitris Bertsimas
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2401.12181
- **Tags:** Universal Neurons, GPT-2, Cross-Seed Replication, Circuit

## Introduction

不同随机种子训练的同架构 GPT-2 是否会学到相同 neuron/feature，是 interpretability 可迁移性的基础。论文研究 individual neuron universality：跨五个 GPT-2 seed，哪些 neuron 在相同输入上持续激活、是否有清晰语义和简单 downstream circuits。

## Method / Framework

作者对 100M tokens 上不同 seed 的 neuron activation 做 pairwise correlation，建立 universality threshold 与 random baseline；再分析 universal neurons 的 weights、activation frequency、skew/kurtosis、词表方向和 causal intervention。进一步按 activation/weight effects 把它们归入 unigram、alphabet、previous-token、position、syntax、context 和 token-suppression families。

## Baselines / Comparisons

比较 GPT2-small/medium 不同 seed、random correlation baseline、非 universal neurons 和 activation-universality vs weight/effect universality。评价包括 excess correlation、feature interpretability、layer distribution、直接 token logit effect 和 neuron intervention，而不是只看 model-level perplexity。

## Experiments / Findings

只有约 1%-5% neurons 超过 universality threshold，远高于随机但说明大多数 basis 不会逐 neuron 对齐；这些 universal neurons 通常有清晰语义。晚层存在预测/抑制连贯 token 集的 neuron family，抑制 neurons 往往比预测 neurons 更晚；还观察到 integer/year、pronoun、position、syntax 和 context roles，以及可因果改变 next-token entropy 的 neuron。

## Ablation / Error Analysis

activation correlation 只找 input-response similarity，weight/effect analysis 发现另一种更抽象 universality；单看相关会漏掉实现相同功能但 basis 不同的 neurons。threshold、token sample、layer pairing、polysemanticity 和 seed 数会影响 1%-5% 估计，intervention 还需控制输出分布改变。

## Limitations

实验选择同架构、同数据 GPT-2，是测 universality 的有利条件；更大模型、不同 tokenizer/数据、SAE feature 和 algorithmic circuits 可能更普适或更不对齐。individual neuron universality 不等于完整 network circuit universality。

## 两句话总结

论文发现同架构 GPT-2 不同 seed 中只有少数 neuron 逐输入复现，但这部分通常具有清晰的词法、语法、位置或 token-logit 功能。它说明可迁移机制可能存在于少量 universal units，也提醒我们不能把 neuron-level 对齐当作所有模型表示都相同。

## Evidence note

本笔记逐段核对 arXiv PDF 的 universality definitions、100M-token correlations、neuron families、weight/effect analysis 与 limitations。

