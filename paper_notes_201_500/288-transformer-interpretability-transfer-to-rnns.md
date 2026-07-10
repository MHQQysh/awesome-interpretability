# 288. Does Transformer Interpretability Transfer to RNNs?

- **Authors:** Gonçalo Paulo, Thomas Marshall, Nora Belrose
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2404.05971
- **Tags:** RNN Interpretability, Mamba, RWKV, Steering, Tuned Lens

## Introduction

Mamba、RWKV 等 recurrent/state-space models 在语言建模上接近 transformer，但其 compressed recurrent state 没有 token-by-token attention，已有 transformer interpretability 工具是否还能工作尚不清楚。本文测试 contrastive activation addition、tuned lens 和 latent knowledge elicitation 能否迁移到 RNN/Mamba/RWKV。

## Method / Framework

作者在 RNN 的 hidden/state trajectory 上构造 contrastive activation vector，比较不同 multiplier、layer/state position 的行为 steering；用 tuned lens 把中间 recurrent state 映射到 token prediction；再在模型被 fine-tune 生成 false outputs 的场景中测试 latent knowledge elicitation。RNN 的 compressed state 既是限制，也可能让一个 state 包含更集中的信息。

## Baselines / Comparisons

比较 Mamba、RWKV 与 Llama 2 等 transformer，采用相同 behavior datasets/prompts 和 steering multipliers；基线是 no steering、不同 layer 的 maximum effect、tuned lens 在 transformer 的已知表现，以及不利用 recurrent compression 的直接迁移。

## Experiments / Findings

多数工具可以 out-of-the-box 迁移到 RNN，得到与 transformer 类似但不完全相同的 steering/lens/latent knowledge 结果。middle layers 的 steering effect 最大；RWKV-5 7B 的响应较低但更平滑，Llama 2 3B effect 较大且可能 non-monotonic。利用 compressed state 的结构可以改进部分 activation addition，但 RNN 的状态更新使时间位置和 multiplier sensitivity 更明显。

## Ablation / Error Analysis

layer/state location、positive/negative multiplier 和 model family 是核心消融；只看最终 state 会错过早期可控方向，直接套 transformer 的 lens 也可能产生 calibration error。论文没有进入 circuit-level head/edge analysis，因此 representation steering 的成功不能说明 RNN 内部算法已经被反向工程。

## Limitations

研究只覆盖 steering、tuned lens、latent knowledge 三类工具，不包括完整 circuit discovery；模型与任务有限，Mamba/RWKV 的版本变化也可能改变结果。RNN compressed state 的可解释性优势尚未转化为自动、稳定的 causal circuits。

## 两句话总结

论文发现 tuned lens、activation addition 和 latent knowledge elicitation 大体能从 transformer 迁移到 Mamba/RWKV 等 RNN，但效果层依赖、强度响应和稳定性不同。它说明 interpretability 工具的抽象原则具有架构迁移性，同时提醒不能把表示级成功当作完整机制解释。

## Evidence note

本笔记逐段核对 arXiv PDF 的 cross-architecture setup、steering figures、tuned lens/false-output experiments 与 conclusion。

