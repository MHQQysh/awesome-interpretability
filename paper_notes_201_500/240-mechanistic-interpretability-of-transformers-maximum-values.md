# 240. Mechanistic Interpretability of Transformers: Extracting Maximum Values from Lists

- **Authors:** Kaushal Thaker
- **Venue:** EngrXiv 2025
- **Paper:** https://engrxiv.org/preprint/download/5115/8678/7218
- **Tags:** Synthetic Task, Transformer Circuits, Attention, Algorithmic Interpretability

## Introduction

用真实语言任务反向工程 Transformer 容易被数据偏差和复杂语义干扰；合成 maximum-of-list 任务可以直接比较模型算法与人类算法。论文训练小型 Transformer 找出变长列表最大值，再观察 attention pattern、encoding 和模型规模如何实现这一算法。

## Method / Framework

从零训练不同层数/头数/隐藏规模的 Transformer，输入 variable-length numeric lists，输出 maximum value。通过 attention map、residual/encoding 和错误案例检查模型是否扫描候选、比较数值或采用 shortcut，并比较 single-head/single-layer 与更大模型的内部策略。

## Baselines / Comparisons

对照不同 depth、head count、model size、sequence length 和训练设置；指标是 maximum extraction accuracy、泛化到列表长度/数值分布的表现，以及 attention/encoding 的机制可读性。实验还观察是否出现 grokking 与单头模型的限制。

## Experiments / Findings

- 小 Transformer 能学会最大值任务，但单层单头模型的 attention/encoding 显示出明显容量限制；增加 head/layer 会改变模型解决算法的方式。
- 作者未观察到清晰 grokking，但可通过分析权重和 attention 追踪候选比较与输出选择，为复杂 LLM 机制研究提供受控起点。
- 结果强调即使简单算法任务也需要同时看 accuracy、长度泛化和内部路径，单看训练集拟合不能说明模型学到了可迁移算法。

## Ablation / Error Analysis

消融层数、头数、列表长度、数字编码和模型大小。容量不足会造成边界位置/最大值相近时失败；attention 可读图不一定是唯一算法，多个 head 可能分工或使用 shortcut，需更多 weight/circuit analysis 验证。

## Limitations

这是 synthetic toy task，不能直接外推到自然语言推理、长上下文或 frontier model；实验没有完全 reverse-engineer 每个 head 的权重功能，也未系统研究 grokking。最大值算法的可解释性主要是教学价值。

## 两句话总结

论文用可控的 maximum-of-list 任务训练小 Transformer，并通过 attention/encoding 和模型规模消融观察其算法电路。它说明 toy task 能暴露层头容量和泛化问题，但距离解释真实 LLM 的复杂行为仍很远。
