# 231. Mechanistic Interpretability for Transformer-based Time Series Classification

- **Authors:** Matías Kalnare, Sofoklis Kitharidis, Thomas Bäck, Niki van Stein
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2511.21514
- **Tags:** Mechanistic Interpretability, Time Series, Activation Patching, Attention

## Introduction

时间序列 Transformer 的 input attribution 只能说明哪些时间点相关，却不说明内部 attention head、layer 和 timestep 如何形成分类。论文把 NLP 机制解释工具迁移到 time-series classification，研究小样本模型是否存在可干预、可画成 causal graph 的离散路径。

## Method / Framework

在 JapaneseVowels speaker-identification TST 上训练 Transformer，使用 activation patching 把 clean run 的 layer/head/timestep state 注入 misclassified run，观察 true-class probability 变化；同时分析 attention saliency、SAE features 和 patching 结果，合成从输入 timestep 到 attention head 再到 class logit 的因果图。

## Baselines / Comparisons

模型 baseline test accuracy 为 97.57%，选择一个 clean 高置信样本和一个错误样本做 denoising patching。解释对照是 input attribution、attention-only、不同 layer/head/timestep patch，指标为 true-class probability change ΔP、恢复正确分类的比例与路径一致性。

## Experiments / Findings

- JapaneseVowels 有 270 train/370 test、9 类、25 timestep、12 维输入；模型高准确率保证 patch effect 不是随机噪声。
- patching 单个 attention head 甚至特定 timestep output 可以明显恢复错误样本，说明 TST 内部存在可操纵的 discrete circuits，而不是所有信息均匀分布。
- causal graph 显示非线性 synergy/interference：某些 head 只有和特定 timestep/其他 head 配合才有效，单一 attribution 排名可能漏掉这种组合机制。

## Ablation / Error Analysis

消融 layer/head/timestep、clean-corrupt pairing、SAE/attention attribution 和 noise strength。噪声过小使 patching 接近无效，过大则恢复困难；单层单头解释会错过协同，padding/时间对齐错误也会制造伪因果路径。

## Limitations

只有一个音频时序数据集和一个小 Transformer，不能代表金融、医疗或长序列 TST；patching 恢复输出是因果证据但仍依赖 corruption 设计。SAE feature 的语义和跨模型 circuit 泛化尚需验证。

## 两句话总结

论文证明 activation patching、attention saliency 和 SAE 可以迁移到时序 Transformer，并在 JapaneseVowels 中定位层、head、timestep 级可干预路径。结果揭示组合 circuit 与非线性协同，但单数据集和 corruption 依赖限制了通用结论。
