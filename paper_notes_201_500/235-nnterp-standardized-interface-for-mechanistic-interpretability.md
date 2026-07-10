# 235. nnterp: A Standardized Interface for Mechanistic Interpretability of Transformers

- **Authors:** Clément Dumas
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2511.14465
- **Tags:** Interpretability Tooling, NNsight, Transformer Interface, Reproducibility

## Introduction

mechanistic interpretability 研究常在 TransformerLens 的统一接口和 HuggingFace/NNsight 的精确原始实现之间取舍：前者需要每个架构手写适配，后者保留行为却缺少标准化。nnterp 要解决的是工具层的复现与可迁移性，而不是提出新的 attribution 算法。

## Method / Framework

nnterp 是包在 NNsight 之上的轻量 wrapper，提供统一的 Transformer 分析接口，同时直接调用原始 HuggingFace implementation。它用 systematic validation 检查 wrapper 输出与原模型的数值一致性，并支持多架构、远程 NDIF 执行和常见的 activation/attention/logit 操作。

## Baselines / Comparisons

对比 TransformerLens 的一致接口但手写架构适配、NNsight 的精确访问但缺少统一抽象，以及直接 HuggingFace hook 的工程方式。比较维度是 numerical fidelity、支持架构数量、API 一致性、迁移代码量、远程运行和研究复现，而非下游 accuracy。

## Experiments / Findings

- validation 表明 nnterp 能在保留 HuggingFace 行为的同时提供跨模型统一操作，减少因自定义重实现带来的数值 mismatch。
- 标准化 API 使 activation patching、logit lens、attention inspection 等实验更容易在不同架构上复用；远程 NDIF 支持把大模型分析从本地显存限制中解耦。
- 工具价值在可复现性：研究者可共享分析脚本而不是每次重新处理 model-specific tensor names 和 hooks。

## Ablation / Error Analysis

工具验证关注 wrapper 与原始模型的层、cache、attention 和 output 对齐；错误主要来自新架构的模块命名、量化/远程执行、dynamic cache 和特殊 forward path。接口统一可能隐藏架构差异，不能把同名张量误认为相同机制。

## Limitations

nnterp 仍依赖 NNsight/HuggingFace 对模型的支持，新架构需要持续适配；标准 API 不会自动解决解释正确性、显存或实验设计。远程执行会引入版本、网络和隐私管理问题。

## 两句话总结

nnterp 用 NNsight 包装保真的 HuggingFace forward path，提供跨 Transformer 架构的统一 mechanistic interpretability 接口。它解决的是工具复现和工程适配瓶颈，但接口一致性本身不等于机制解释一致。
