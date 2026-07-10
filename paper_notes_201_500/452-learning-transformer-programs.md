# 452. Learning Transformer Programs

- **Authors:** Dan Friedman, Alexander Wettig, Danqi Chen
- **Venue / year:** NeurIPS 2023
- **Paper:** [arXiv:2306.01128](https://arxiv.org/abs/2306.01128)
- **Tags:** `intrinsic-interpretability` `RASP` `program-induction` `transformer-circuits`

## Introduction
后验 reverse-engineering 需要大量人工工作，且通常只能给出部分、未必忠实的解释。作者反过来问：能否直接训练一种被结构约束、训练后可自动还原成程序的 Transformer，从源头降低解释难度。

## Method / Framework
方法以 RASP/Tracr 为灵感，但不是把人工程序编译成网络，而是让受约束 Transformer 用梯度学习程序。残差流被拆成正交的命名变量；投影矩阵由 one-hot gate 决定每个模块读取哪个变量；模块输出写入新的专用地址。类别 attention 用可学习的 query-key predicate 和 hard `select_closest`，数值 attention 聚合计数，MLP 被限制为可枚举的 lookup-table。

离散 gate 和 predicate 用 Gumbel-Softmax 连续松弛训练，逐步退火温度；训练后取最大似然离散权重，把每个 head 确定性地 decompile 成 Python/RASP 风格程序。这样程序与网络功能等价，却可断点调试、静态分析和追踪子程序。

## Baselines / Comparisons
实验包括 toy in-context learning、RASP 的 Reverse/Histogram/Sort/Dyck 等算法任务、CoNLL-2003 NER，以及 TREC/MR 等文本分类。算法任务与标准 Transformer 比较；NER 还与 unigram baseline、同样用 GloVe 初始化的 standard Transformer 比较。

## Experiments / Findings
toy ICL 学到两层 induction-head 结构：第一头把前一位置的 token 写入中间变量，第二头按当前字母匹配此前出现过的字母并复制其后数字。RASP 七项任务中 Reverse 99.79、Histogram 100.0、Double-Histogram 98.40、Sort 99.83、Most-Freq 75.69、Dyck-1 99.30、Dyck-2 99.09；五项超过 99%。CoNLL NER 中 unigram F1 66.2，standard Transformer 70.5，Transformer Program 76.1。

## Ablation / Error Analysis
短序列上程序有效，但序列长度和词表增大时，Transformer Program 比标准 Transformer 退化更快，最难设置出现超过 50 个百分点差距。Sort 程序约 300 行，包含位置启发式和跨层组合，说明自动学到的程序不一定是人类最简算法；Most-Freq 和长输入失败暴露了离散优化及固定变量容量的脆弱性。

## Limitations
离散结构限制了表达能力和扩展规模；当前程序族主要覆盖可映射为 RASP 原语的操作。与普通 Transformer 相比性能和 scaling 仍有明显代价，且“可读程序”本身可能是启发式实现，不保证是唯一或最自然的算法。

## 两句话总结
论文提出一种 intrinsic interpretable Transformer：通过正交变量、离散 attention predicate 和 Gumbel-Softmax 学习，让网络训练后自动还原为等价 Python 程序。它在 toy ICL、算法任务和 NER 上显示了可解释性与可用性的平衡，但长序列和复杂任务上的性能退化仍是主要瓶颈。

## Evidence note
已读取正文中的框架约束、Gumbel 优化、四类实验、表格结果和 scaling/error discussion。
