# 243. Decoding Vision Transformers: the Diffusion Steering Lens

- **Authors:** Ryota Takatsuki, Sonia Joseph, Ippei Fujisawa, Ryota Kanai
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2504.13763
- **Tags:** Vision Transformer, Diffusion Lens, Steering, Mechanistic Interpretability

## Introduction

Logit Lens 把语言模型中间表示投到词表，帮助观察逐层预测，但 ViT 的视觉表示不是词汇空间，直接套用难以表达 patch、head 和 submodule 的差异。论文提出 Diffusion Steering Lens (DSL)，用 representation steering 和 submodule patching 将 ViT 中间贡献投影到可比较的视觉输出空间。

## Method / Framework

DSL 对 ViT 的中间 residual/submodule representation 做 controlled steering，测某个 attention head/MLP 的输出如何改变最终视觉 representation 或 class similarity，再结合 patching 分离各 submodule 的直接贡献。与只将中间状态直接 decode 的 Diffusion Lens 不同，DSL 关注干预后的 causal response。

## Baselines / Comparisons

比较 Logit Lens、Diffusion Lens、attention/head similarity、submodule patching 与 DSL；指标包括 output cosine similarity、steering effect、head-level contribution concentration 和不同 layer/module 的可重复性。

## Experiments / Findings

- DSL 对 individual submodule 的 effect 更集中，比 Diffusion Lens 更能区分真实贡献；高 head-output similarity 不总意味着 patch 后输出损失大，揭示 Hydra effect/冗余路径。
- layer/head 级响应显示 ViT 的视觉信息不是单一路径，而是多个 submodule 共同塑造；steering lens 可将其贡献解码成更容易比较的视觉空间。
- 结果支持把 Logit Lens 的“层级预测”转成视觉 Transformer 的“层级响应/干预”，但仍是 proxy 而非完整语义解释。

## Ablation / Error Analysis

消融 steering strength、patch location、head/submodule 类型和输出相似度指标。过强 steering 会改变 representation content 而不仅是读出，低相似度输出也可能因 redundancy 仍保持分类，导致贡献估计不唯一。

## Limitations

DSL 需要白盒 ViT 和精心设计的 representation target，实验任务/模型有限；视觉输出空间的选择影响解释。作者也指出 steering 可能意外改变内容，不能把 lens visualization 当作单一因果 circuit。

## 两句话总结

DSL 用 representation steering 与 submodule patching 改造 Vision Transformer 的 Logit Lens，使中间视觉贡献可以被逐层比较并更接近因果效应。它减少直接 decode 的语义缺失，但 steering 本身可能改变表示，冗余路径也让贡献不唯一。
