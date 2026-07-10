# 151. Scaling Vision-Language Models with Sparse Mixture of Experts

- **Authors:** Sheng Shen, Zhewei Yao, Chunyuan Li, Trevor Darrell, Kurt Keutzer, Yuxiong He
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.758
- **Tags:** Vision-Language, Sparse MoE, Routing, Scaling

## Introduction

大规模 vision-language model 能处理 VQA、视觉推理和图文检索，但 dense 模型的参数量、训练和部署成本很高。MoE 可以增加总参数而只激活少量 expert；现有 multimodal MoE 多偏对比学习或 vision-only benchmark，不能说明它在生成式视觉语言任务上如何扩展，也不清楚稀疏 routing 是否让模型行为更可解释。

## Method / Framework

作者提出 VL-MoE，在视觉、文本和跨模态 feed-forward 层引入 sparse experts，并用 modality-aware router 根据 token 选择 V-FFN、T-FFN 或 VL-FFN。模型保留共享 attention，只有 top-k experts 参与计算，从而增加参数容量而控制每 token FLOPs。

训练覆盖 image-text contrastive、image-text matching / generation 等任务；为防止 expert load imbalance，加入 auxiliary load-balancing loss。作者还记录不同输入 token 的 routing，分析 image / text / multimodal expert 是否形成可解释的 specialization。

## Baselines / Comparisons

与 dense vision-language Transformer、LIMoE、不同 expert 数、不同 modality routing 和不同规模 VL-MoE 比较。下游任务包括 VQA、NLVR2、image-text retrieval，以及视觉分类 / zero-shot transfer；指标为 VQA score、NLVR2 accuracy、retrieval recall 和分类 accuracy，同时报告参数、显存、吞吐。

## Experiments / Findings

- 在相同或相近计算成本下，VL-MoE 在多项 vision-language benchmark 上超过 dense baseline，证明 MoE 扩容不仅适用于纯文本。
- expert 数增加时性能总体上升，但收益受 routing 稳定性和负载均衡约束；专家池太大而没有足够 auxiliary loss 会产生 token dropping 和训练崩溃。
- 视觉 token、文本 token 与跨模态 token 的 routing 分布不同，说明 expert 学到了一定 modality specialization；这种路由比 dense FFN 更容易提供行为层面的解释。
- auxiliary loss 对训练稳定性关键；没有它，少数 expert 会吸收大量 token，导致其它 expert 没有训练信号和最终性能下降。
- 与 LIMoE 的比较显示，仅有 sparse multimodal architecture 不保证随规模稳定提升，生成式目标和路由设计共同决定收益。

## Ablation / Error Analysis

消融包括 expert 数、expert pool 规模、共享 / 分离 modality experts、auxiliary loss 和不同 scaling strategy。主要失败来自 load imbalance、capacity overflow、token dropping 和大模型路由噪声；某些任务更偏好 modality-specific expert，另一些需要共享 cross-modal expert，因此完全隔离专家也会损害 transfer。

## Limitations

论文仍需要大规模训练资源，VLM routing 的可解释性只是 token-to-expert 相关性，不等于 expert 内部学到清晰概念。模型主要是 2023 年的视觉语言架构，不能直接推断今天的 decoder-only multimodal LLM。路由还受 batch、capacity 和 auxiliary loss 影响，跨数据集稳定性需要更多研究。

## 两句话总结

VL-MoE 用稀疏、模态感知的 experts 扩大视觉语言模型容量，并通过 routing 分析展示了视觉、文本和跨模态 token 的一定 specialization。实验说明 MoE 能在相近计算下提升多项 VLM 指标，但负载均衡、token dropping 和路由解释的因果性仍是核心问题。
