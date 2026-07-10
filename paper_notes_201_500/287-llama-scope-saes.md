# 287. Llama Scope: Extracting Millions of Features from Llama-3.1-8B with Sparse Autoencoders

- **Authors:** Zhengfu He, Wentao Shu, Xuyang Ge, Lingjie Chen, Junxuan Wang, Yunhua Zhou, Frances Liu, Qipeng Guo, Xuanjing Huang, Zuxuan Wu, Yu-Gang Jiang, Xipeng Qiu
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2410.20526
- **Tags:** SAE Scaling, Llama-3.1, Feature Dictionary, Open Interpretability Tools

## Introduction

SAE 解释大模型的瓶颈不是单个 demo，而是每一层、每个 sublayer、不同上下文长度都缺少可用的 feature dictionary。Llama Scope 的问题是能否在 Llama-3.1-8B-Base 上规模化训练、发布和解释数百万 features，并检查 base SAE 是否能迁移到 longer context 与 fine-tuned model。

## Method / Framework

项目训练 256 个 TopK-ReLU SAEs，覆盖每层和 R/A/M/TC 等位置，提供 32K 与 128K width，最高 expansion 约 36.6x；改进 TopK SAE 的训练与稀疏约束，发布 checkpoints、scalable training、interpretation 和 visualization tools。作者用 feature activation geometry、feature splitting 和跨 context/checkpoint reconstruction 评估 dictionary，而不是只发布权重。

## Baselines / Comparisons

比较 Gemma Scope、GPT-4/Claude 3 的相关 SAE 项目、不同 SAE width/position/activation function 和 base vs fine-tuned/long-context activation。指标包括 L0、reconstruction/CE recovery、feature interpretability、geometry 与 feature splitting；重点是资源与可复现性，不是某个 downstream accuracy SOTA。

## Experiments / Findings

Llama Scope 让 Llama-3.1-8B 的每层 feature dictionary 规模达到 millions，且可在较大 width 下维持稀疏表示。geometry 分析支持 feature splitting：dictionary 变宽会发现更细的新 feature，而非简单复制；base SAEs 在 longer context/fine-tuned model 上具有一定 generalization，但位置与分布偏移会影响重构和解释质量。

## Ablation / Error Analysis

width、TopK、SAE position 和 context distribution 是主要消融。width 越大可能提高语义细分但增加死 feature、训练和解释成本；只看 activation examples 可能误判 feature，需结合 reconstruction、causal steering/ablation。base-to-chat transfer 不能保证 chat safety feature 已被捕获，尤其是 fine-tuning repurpose 的 latent。

## Limitations

256 个 SAE 的训练和人工解释成本仍高，百万 feature 需要自动 annotation 但其可靠性有限；公开 suite 主要是 Llama-3.1-8B base，跨架构 universality 仍需单独实验。SAE feature 不是模型完整 circuit，不能仅凭 dictionary 解释生成算法。

## 两句话总结

Llama Scope 把 SAE 从单层实验扩展为 Llama-3.1-8B 全层、256 个 checkpoint 和百万 feature 的开放工具链，并系统观察 feature splitting。它降低了大规模 MI 的入口成本，但 reconstruction、自动解释、上下文迁移和 feature 到 circuit 的距离仍然存在。

## Evidence note

本笔记逐段核对 arXiv PDF 的 suite design、Table 1、TopK-ReLU training、cross-context/fine-tuning evaluation 与 geometry analysis。

