# 274. Towards Principled Evaluations of Sparse Autoencoders for Interpretability and Control

- **Authors:** Aleksandar Makelov, George Lange, Neel Nanda
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2405.08366
- **Tags:** Sparse Autoencoder, Feature Dictionary, IOI, Sufficiency, Necessity

## Introduction

SAE 把 activation 分解成稀疏 feature，常被用于解释和控制，但现实中没有 ground-truth feature dictionary，重构 loss 或 feature sparsity 不能证明 feature 真有语义、能控制模型。论文提出更原则化的评估：用有监督 feature dictionary 作为 task-level reference，分别测 approximation、control 和 interpretability。

## Method / Framework

作者在 GPT-2 Small 的 IOI task 上构造 supervised dictionary，使 feature 对应已知的 name mover、duplicate token、position 等任务变量；然后与 IOI-trained SAE、OpenWebText SAE 对比。评价包括 reconstruction approximation、feature ablation/activation control 的 sufficiency/necessity、feature attribution/interpretability；还研究 SAE 的 feature occlusion 与 feature splitting 等训练现象。

## Baselines / Comparisons

比较对象是 supervised dictionary、IOI SAE、OpenWebText SAE、PCA/重构相关 baselines 和原始 model activation。关键不是哪个 SAE loss 最低，而是“它能否复现并干预 task computation”；因此作者分别报告 approximation、control 和 interpretability，避免单指标排序。

## Experiments / Findings

supervised dictionary 在 IOI 上能很好近似、控制和解释 computation；SAE 也能捕捉可读的 IOI features，但在 control 上弱于 supervised features，说明 feature label 可读不代表 activation replacement faithful。task SAE 通常比 full-distribution SAE 更贴合 IOI，但会引入任务偏置。训练中观察到 feature occlusion：因果相关 concept 被略高幅度 feature 遮蔽；也观察到 feature splitting，单个语义方向可能被多个 SAE feature 分摊。

## Ablation / Error Analysis

把 SAE feature 当作完整坐标、仅比较 reconstruction 或仅看 top activating tokens 都会高估解释质量。feature dictionary 的 sufficiency/necessity test 依赖 activation corruption 与任务范围；若 task 不覆盖某概念，不能 conclude disentanglement。SAE 的 sparsity、dictionary width、training distribution 和 feature selection 会改变结果，且 control test 比语言描述更严格。

## Limitations

supervised dictionary 只提供 task-local ground truth，不是自然语言模型全分布的真实特征；IOI 是小型 circuit，结论不能直接推广到大规模开放域 SAEs。自动 interpretability 仍可能误标 feature，control 评价也受 intervention distribution 影响。

## 两句话总结

这篇工作提出用 supervised feature dictionary 作为参照，同时评价 SAE 的重构、可控性和可解释性，避免把低 loss 当作解释成功。IOI 实验显示 SAE 能学到可读 feature，但控制能力弱于有监督特征，并揭示了 occlusion/splitting 等 SAE 学习现象。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Methods、IOI evaluations、task/full-distribution SAE experiments 与 qualitative phenomena。

