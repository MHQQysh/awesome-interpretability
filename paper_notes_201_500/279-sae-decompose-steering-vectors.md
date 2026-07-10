# 279. Can Sparse Autoencoders Be Used to Decompose and Interpret Steering Vectors?

- **Authors:** Harry Mayne, Yushi Yang, Adam Mahdi
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2411.08790
- **Tags:** Sparse Autoencoder, Steering Vectors, Activation Steering, Negative Features

## Introduction

Activation steering 通过在推理时添加 steering vector 改变 sycophancy、harmlessness、refusal 等行为，但 steering vector 的内部语义通常不透明。直觉上可以把它送进 SAE，看哪些 sparse features 被激活；论文检验这个直觉是否成立，并解释为什么 SAE reconstruction 有时失去原 steering ability。

## Method / Framework

作者把不同层、不同 steering direction 输入 SAE，比较原 vector、SAE latent decomposition、SAE reconstruction 的行为效果和 top features；同时分析 steering vector 与 SAE training input distribution 的距离、feature projection 的正负号和模型行为在 layer 上的变化。实验包含 instruction-tuned Llama 2 7B 等设置，并将 steering 强度、层位置和 zero/baseline 对照结合。

## Baselines / Comparisons

核心 baseline 是直接使用原 steering vector、zero/no steering、SAE reconstructed vector，以及把 steering vector 分解后只保留正向或 top-k features。比较指标是 steerability/behavior change、重构误差、top activating feature 的语义一致性和不同层的效果，而非仅看 SAE loss。

## Experiments / Findings

直接 SAE reconstruction 经常无法保留原 steering behavior，原因有二：steering vector 可能落在 SAE 训练分布之外，SAE 在该区域的 reconstruction 不可靠；steering vector 在有意义的 feature direction 上可能有 negative projection，而常见 non-negative SAE/feature interpretation 不能自然表达“抑制某 feature”。steerability 通常在模型中间层更高，说明 steering direction 与层级表示/行为 readout 有关，不是任意层都等价。

## Ablation / Error Analysis

只取 top positive features 会丢掉负向成分，重构后可能把 steering 变成另一种行为；只看 cosine reconstruction 也不能保证 behavioral equivalence。把 vector 投影到 activation manifold、改变 SAE feature sign/输入分布和比较 middle layers 是关键诊断。SAE feature 名称看起来合理，不代表该 feature 是 steering mechanism 的唯一因果变量。

## Limitations

结果依赖 SAE 架构、训练分布、层和 steering task；不能据此否定所有 SAE-based steering interpretation。负向 feature 处理与 out-of-distribution dictionary learning 仍需新方法，且行为测试覆盖的 steering directions 有限。

## 两句话总结

论文证明把 steering vector 直接交给 SAE 可能产生误导，因为 vector 常在 SAE 训练分布外且包含重要负向 feature projection。它把“可解释分解”与“保留 steerability”分开，提醒研究者必须做行为重构验证而不能只展示 top feature。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、Sections 3-4、Tables 2-3、layer/negative-feature analysis 与 discussion。

