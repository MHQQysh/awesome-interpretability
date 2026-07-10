# 411. Finding Neurons in a Haystack: Case Studies with Sparse Probing

- **Authors:** Wes Gurnee, Neel Nanda, Matthew D. Pauly, Katherine Harvey, Dmitrii Troitskii, Dimitris Bertsimas
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2305.01610
- **Tags:** `sparse-probing` `neurons` `polysemanticity` `mechanistic-interpretability`

## Introduction
研究者常希望定位“哪个 neuron 表示某个概念”，但 dense probe 可以组合大量神经元，得到高准确率却不提供精确定位；单看最高激活样本又容易制造 interpretability illusion。本文提出 sparse probing，用最多 k 个 neuron 的简单分类器系统研究 100 多个 linguistic/factual/context features。

## Method / Framework
论文对 MLP neuron activations 训练 k-sparse logistic probes，包含 1-sparse、adaptive thresholding 和 optimal sparse probing (OSP)。使用 F1、precision、recall 评价定位；再对候选 neuron 的输入激活、输出 logit effect 和全语料分布做二次检查，并对单 neuron 做 ablation。

## Baselines / Comparisons
对比 dense probes、difference-in-means、mutual information、系数排序和随机 neuron。实验覆盖语言、代码、compound words、职业/事实属性等 feature，并比较 Pythia/GPT-like 模型的不同规模和层。

## Experiments / Findings
早期层常把大量 feature 压缩进 sparse combinations of polysemantic neurons，体现 superposition；中间层出现更多看似 dedicated 的 context/feature neurons。模型规模增大后，一些 feature 获得专门 neuron，同时另一些 feature split 成更细粒度的表示，说明 scaling 同时增加容量和语义分化。

## Ablation / Error Analysis
对 French、Go code、context disambiguation 和 factual features 的 neuron ablation 会增加 language-model loss，支持候选 neuron 的功能相关性。但高 F1 不等于 monosemantic：一个 neuron 可能只在 probe dataset 上纯，在更广 corpus 上响应无关 n-gram。feature label 的负例采样、罕见事件和 tokenization 会强烈影响 precision/recall。

## Limitations
probe 是读出器，不证明模型计算依赖该 neuron；单一 neuron 也不是唯一或完整 circuit。证明 monosemantic 需要极大覆盖的输入分布，稀有响应容易漏掉。研究主要是 decoder LM 的局部神经元，不能代替 SAE/跨层机制分析。

## 两句话总结
Sparse probing 用容量受限的线性探针定位语言模型中的 feature-neuron 对，显示早期层的 superposition/polysemanticity 与中间层的 dedicated neurons 并存。它比 dense probing 更适合定位，但 probe dataset、稀有特征和全语料检查决定了“可解释 neuron”是否真实。

## Evidence note
已读取 arXiv 2305.01610 v2 本地 PDF 的 sparse probing、100+ features、superposition、scale、ablation 和误差讨论。
