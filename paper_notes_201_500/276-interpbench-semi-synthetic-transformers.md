# 276. InterpBench: Semi-Synthetic Transformers for Evaluating Mechanistic Interpretability Techniques

- **Authors:** Rohan Gupta, Iván Arcuschin, Thomas Kwa, Adrià Garriga-Alonso
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2407.14494
- **Tags:** Mechanistic Interpretability Benchmark, SIIT, Tracr, Circuit Discovery

## Introduction

MI 方法要找出模型真实算法，但真实模型没有 ground-truth circuit，方法成功与否很难验证。InterpBench 的问题是能否生成“足够像 transformer、但 circuit 已知”的 benchmark，让 circuit discovery、ablation 和 feature attribution 有明确答案。

## Method / Framework

作者提出 Strict Interchange Intervention Training (SIIT)，在原 IIT 基础上不仅对齐模型和目标 high-level causal model 的内部计算，还阻止非-circuit nodes 影响输出。以 Tracr 生成的 sparse transformers 为基础，SIIT 训练保持已知 circuit，同时可扩大到 IOI 等更复杂 circuit，形成 semi-synthetic but realistic transformers。评测中用 known circuit 作为 ground truth，比较发现方法是否找到正确 nodes/edges。

## Baselines / Comparisons

InterpBench 覆盖 Tracr/ SIIT models、IOI circuit 以及 69 个新增任务；对照包括常见 circuit discovery、resample/mean/zero ablation、node-in-circuit 与 out-of-circuit intervention、以及随机/非电路组件。指标包含 circuit node effect、ROC-AUC、准确率、KL divergence、Wilcoxon-Mann-Whitney 和 Vargha-Delaney effect size。

## Experiments / Findings

SIIT models 保留 Tracr 原始 circuit，同时比原始合成网络更接近真实 transformer 的分布；它也能训练带更大 IOI circuit 的模型。benchmark 说明不同 discovery 方法在“定位输出相关节点”和“恢复真实 causal circuit”之间有差异，且在最差情况下即使方法在小 Tracr 任务上有效，也可能无法对复杂 IOI circuit 稳定工作。

## Ablation / Error Analysis

严格禁止 non-circuit nodes 影响输出是关键，否则模型可学到 shortcut，导致 circuit evaluation 虚高。mean ablation 与 zero ablation 的差异说明 distribution shift 会影响节点重要性；resample ablation 更接近保持输入分布。任务类型、circuit 稀疏度、节点数量和 intervention 选择都会影响 AUC，因此 benchmark 结果不能脱离 ablation protocol 比较。

## Limitations

semi-synthetic circuit 仍比自然语言 LLM 简单，known causal model 也不保证覆盖真实模型中的冗余/等价实现。InterpBench 评估的是 MI tooling，不直接说明这些方法能在 GPT-4 等闭源大模型上工作；训练 SIIT 模型本身也需要指定目标 circuit。

## 两句话总结

InterpBench 用 SIIT 训练内部 circuit 已知、外观更像真实 transformer 的模型，解决了 MI 方法缺少 ground truth 的验证难题。它让 circuit discovery 可以做可重复的量化比较，但 semi-synthetic 任务与真实自然语言模型之间仍有明显分布差距。

## Evidence note

本笔记逐段核对 arXiv PDF 的 SIIT definition、InterpBench construction、Tables 1-11、ablation 与 conclusion。

