# 232. Sparse Autoencoders Learn Monosemantic Features in Vision-Language Models

- **Authors:** Mateusz Pach, Shyamgopal Karthik, Quentin Bouniot, Serge Belongie, Zeynep Akata
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2504.02821
- **Tags:** SAE, Vision-Language Model, Monosemanticity, Steering

## Introduction

SAE 已用于拆解 LLM residual stream，但 VLM 的 visual representation 同时承载对象、颜色、纹理和组合语义，如何量化 feature 是否单义仍缺少统一标准。论文把 SAE 应用到 CLIP/VLM 视觉表征，建立与人类感知对齐的 MonoSemanticity score，并检验 feature 能否用于 multimodal steering。

## Method / Framework

训练 BatchTopK SAE 与 Matryoshka BatchTopK SAE，对 VLM visual layers 的神经元表示做稀疏字典学习；用 feature top-activating images/概念计算 MonoSemanticity (MS) score，再通过 71 位用户、1000 个问题的 pairwise study 检验 MS 与人类判断的一致性。选定 feature 后进行 unsupervised concept-based steering，观察图文模型输出变化。

## Baselines / Comparisons

比较原始 VLM neurons、BatchTopK SAE、Matryoshka SAE、不同 layer 与 expansion factor；指标包括 MS、sparsity、reconstruction、human alignment 和 steering 行为。用户在成对 image sets 中选择更 monosemantic 的集合，作为自动 MS 的外部验证。

## Experiments / Findings

- 人类选择与 MS 在 82.8% 的问题上相符；当 MS 差异从 0.0--0.1 增大到 0.8--0.9，一致率从 56.6% 升到 100%，说明 metric 与感知单义性有较强对应。
- SAE neuron 的 MS 明显高于原始 neuron；稀疏字典学习本身带来 disentanglement，Matryoshka SAE 在更宽 dictionary 下表现更好。
- 清晰分离的 visual concept 可以作为无监督 steering 方向，为 VLM 安全/可控性提供不依赖标签的接口，但 feature label 与真实因果语义仍需干预确认。

## Ablation / Error Analysis

消融 SAE 类型、层、expansion factor、top feature 和用户评测阈值。更宽字典可能增加 feature splitting/低频孤立 feature，过强稀疏会损失重构；CLIP 的共现语义会让一个 feature 看似单义却只代表背景 shortcut。

## Limitations

实验主要是视觉表征和小规模 steering，不能直接覆盖 VLM language tower、复杂对话或训练中 learned circuits。人类 MS judgment 受图像语义和任务描述影响，且无监督 steering 可能改变无关概念。

## 两句话总结

论文提出 MonoSemanticity score 并用大规模人类选择验证，证明 SAE 能把 VLM visual neurons 拆成更清晰的单义视觉概念。Matryoshka/BatchTopK feature 还可用于 concept steering，但 feature 可读性和因果性仍需更强验证。
