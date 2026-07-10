# 272. Towards Uncovering How Large Language Model Works: An Explainability Perspective

- **Authors:** Haiyan Zhao, Fan Yang, Bo Shen, Himabindu Lakkaraju, Mengnan Du
- **Venue / year:** 2024 arXiv perspective/survey
- **Paper:** https://arxiv.org/abs/2402.10688
- **Tags:** LLM Explainability, Knowledge Composition, Mechanistic Interpretability, Survey

## Introduction

LLM 的 hallucination、toxicity、dishonesty 和 misalignment 使“知道模型内部怎么工作”成为安全问题；传统黑盒解释往往只能描述输入-输出相关性。本文从 explainability 视角整理 LLM 知识如何在参数、神经元、层和 attention circuit 中组成，并讨论这些 insight 如何转化为 detection、editing、alignment 与部署工具。

## Method / Framework

文章以问题驱动的 perspective 组织材料：知识如何编码/组合，模型如何进行推理与泛化，哪些组件负责事实、语义、行为和错误；随后汇总 probing、activation/attention analysis、causal intervention、model editing、SAE 与 mechanistic circuit 等方法。核心区分是“观察/解码某个 representation”与“干预后模型行为按预测改变”，后者更接近解释性证据。

## Baselines / Comparisons

这是综述而非单一实验，比较维度包括 neuron-level 与 feature-level、post-hoc attribution 与 causal intervention、black-box output analysis 与 white-box parameter/activation analysis，以及 knowledge editing、steering、safety monitoring 的不同目标。文章还讨论 GPT、LLaMA-2 等模型与两层 decoder-only controlled experiments，强调真实预训练模型和 toy setting 的证据强度不同。

## Experiments / Findings

文章归纳出几个方向：LLM 知识不是单一 neuron 的静态字典，而会分布在 superposition、MLP key-value memory、attention information routing 和层间组合中；泛化能力使模型可能通过与传统机器学习不同的结构重用知识。解释 insight 可以用于定位 hallucination/toxicity、编辑不良事实、检测 dishonest behavior 和设计更可控的训练/部署流程。

## Ablation / Error Analysis

综述强调 probing 的高解码率不代表模型使用了该 feature，attention weight 也不天然等于重要性；编辑方法若不检查邻近概念会损伤能力，steering 若缺 causal test 可能只是改变输出表面。不同方法的 failure mode、模型规模、训练阶段和 prompt sensitivity 需要在同一任务上控制，不能把单一案例推广为通用机制。

## Limitations

作为 perspective，论文不提供一套统一 benchmark 或新的跨模型因果结论；该领域文献快速变化，survey taxonomy 会随 SAE、attribution graph 和 automated interpretability 发展而更新。真实 LLM 的内部机制远比所覆盖的 toy circuits 复杂。

## 两句话总结

这篇 perspective 把 LLM explainability 从输出解释扩展到知识组成、表示、circuit、干预和可控应用，并梳理了各方法能证明什么、不能证明什么。其主线是把“可解码”与“可因果干预”区分开，避免把相关性 probe 或 attention 图误当成模型算法。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、knowledge composition、mechanistic methods 与应用讨论；综述性结论未伪造单篇实验数值。

