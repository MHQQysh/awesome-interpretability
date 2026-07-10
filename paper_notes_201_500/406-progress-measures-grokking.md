# 406. Progress Measures for Grokking via Mechanistic Interpretability

- **Authors:** Neel Nanda, Lawrence Chan, Tom Lieberum, J. Lacey Smith, Jacob Steinhardt
- **Venue / year:** ICLR, 2023
- **Paper:** https://arxiv.org/abs/2301.05217
- **Tags:** `mechanistic-interpretability` `grokking` `progress-measures` `Fourier`

## Introduction
神经网络有时在训练很久后突然从 memorization 转为 algorithmic generalization，这种 grokking 现象难以用 train/test loss 单独解释。本文在 modular arithmetic 等程序化任务上寻找内部 circuit 和可量化的 progress measures，解释模型何时学会了真正的算法。

## Method / Framework
作者训练小型 Transformer，分析 Fourier features、induction/lookup components、attention 和 MLP 的表示演化。progress measures 跟踪特定频率、特征或 circuit 的强度/损失贡献，配合 parameter ablation、activation/weight visualization 和训练时间曲线，把“突然泛化”拆成可观察的内部阶段。

## Baselines / Comparisons
对比 train loss/test loss、random/未训练模型、不同 weight decay/模型规模和仅 memorization 的早期 checkpoint。研究还比较不同 Fourier modes、attention heads 和 MLP feature 对训练/泛化的贡献，而不是只看最终 accuracy。

## Experiments / Findings
模型先记忆训练样本，随后逐步形成 Fourier/algorithmic circuit，test performance 在内部结构成熟后突然上升。progress measures 能在 test accuracy 变化之前指示模型正在从 memorization 转向 generalization，因此比单一 loss 曲线更早地解释 grokking。

## Ablation / Error Analysis
删除关键 Fourier modes、heads 或权重衰减会阻止/延迟 grokking；不同模块的形成顺序说明算法不是一个瞬间出现的单节点。toy arithmetic 任务的电路比较干净，但在更复杂自然语言任务中相同 progress measure 可能不存在或难以选择。

## Limitations
模型和任务很小，结论不能直接外推到现代 LLM 的 emergent abilities。Fourier circuit 的识别依赖程序化任务和研究者先验；单个可解释电路不保证大模型所有算法都能如此拆分。

## 两句话总结
本文用 Fourier、attention/MLP 电路和 progress measures 跟踪 Transformer 从记忆训练样本到真正算法泛化的 grokking 过程。它说明内部机制指标可以领先于行为指标预告能力跃迁，但 toy task 的清晰电路不代表通用 LLM 已能同样容易解释。

## Evidence note
已读取 ICLR 2023/arXiv 2301.05217 本地 PDF 的 grokking setup、progress measures、circuit ablation 和结论。
