# 475. Successor Heads: Recurring, Interpretable Attention Heads in the Wild

- **Authors:** Rhys Gould, Euan Ong, George Ogden, Arthur Conmy
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2312.09230](https://arxiv.org/abs/2312.09230)
- **Tags:** `attention-heads` `numeric-representation` `SAE` `successor-circuit`

## Introduction
论文寻找在多种架构和规模中反复出现、可机械解释的 attention head。Successor head 的行为是把有自然序的 token（数字、月份、星期）映射到 successor，例如 Monday→Tuesday，并进一步问其内部是否有跨 domain 的抽象 numeric representation。

## Method / Framework
作者分析 embedding→MLP0→OV head→unembedding 的 successor circuit。用线性投影把 MLP0 representation 分为 index-space 与 domain-space；再训练 SAE 找重要 feature，用 linear probe、single-neuron ablation 和 direct-effect mean ablation 验证 feature，最后通过 vector arithmetic 操作 mod-10 feature 并在 Pile 自然文本上追踪 head 的多语义角色。

## Baselines / Comparisons
比较 GPT-2、Pythia、Llama-2（约 31M 到 12B）中的 successor head，比较 SAE feature、linear probe 和 neuron-level 证据，并用随机/非 successor token、不同 ablation method、Roman numerals OOD 作为对照。

## Experiments / Findings
index/domain 分解在 held-out token pairs 上 top-1 output decoding 达 1.00；Roman numerals 只有 I/V/X 等单 token 歧义造成主要错误。mod-10 feature 的 modal feature 平均覆盖同类 58.5% token，probe 与 SAE feature 平均 cosine 0.70764，并可在 succession tasks 的 102 个非数字样例中正确预测 94 个 index mod 10。vector arithmetic 在月份/20-29 数字上分别约 53%/89% 成功。

## Ablation / Error Analysis
successor head 有明显 greater-than bias，导致把较低序 token 往下 steering 时常错误地跳到更高的 token；mod-10 feature 本身不包含这一 bias。feature 在至少两个 neuron 中 superposition，说明可解释 feature 不等于单 neuron；Days/Letters 任务上 steering 失败，表明抽象结构仍不完整。

## Limitations
mod-10 只覆盖 successor 的一部分，训练数据、tokenization 和单 token 假设很重要；自然文本中的 head 还承担四类 polysemantic behavior。direct effect ablation 只在有限 context 上评估，不能把 successor head 当成全模型的唯一算法。

## 两句话总结
Successor heads 在 GPT-2、Pythia、Llama-2 中反复出现，并用可跨 domain 的 index/domain 与 mod-10 features 实现顺序 token 增量。SAE、probe、neuron ablation 和 vector arithmetic 共同支持其机制，但 greater-than bias 与 polysemantic use 说明该电路仍有隐藏成分。

## Evidence note
已读取 successor circuit、index/domain decomposition、SAE/probe/neuron 验证、vector arithmetic、OOD 与自然文本 polysemantic analysis。
