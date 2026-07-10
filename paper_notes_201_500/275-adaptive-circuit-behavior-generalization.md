# 275. Adaptive Circuit Behavior and Generalization in Mechanistic Interpretability

- **Authors:** Jatin Nainani, Sankaran Vaidyanathan, Alexander Seeshing Yeung, Kartik Gupta, David Jensen
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2411.16105
- **Tags:** Circuit Generalization, IOI, DoubleIO, TripleIO, S2 Hacking

## Introduction

经典 circuit 往往在一个窄 prompt format 上发现，然后默认它解释同一任务的其他表达。LLM 却能跨 prompt 泛化；问题是泛化时复用同一组件、同一机制，还是切换到另一套 circuit。论文以 GPT-2 Small 的 IOI circuit 为对象，测试 DoubleIO/TripleIO 等 variant。

## Method / Framework

作者从已知 IOI circuit 出发，把 prompt 改成不同位置、重复次数、顺序和目标结构的 DoubleIO/TripleIO inputs；对 full model 与 base IOI circuit 分别测 logit difference 和 accuracy。随后做 node/edge ablation、circuit discovery 和 faithfulness analysis，检查原 circuit 是否保持必要性/充分性，并定位模型在原算法应失败的 variant 上为何仍成功。

## Baselines / Comparisons

主要对照是 full GPT-2、base IOI circuit、DoubleIO/TripleIO discovered circuits、只保留 nodes 或 edges 的 ablations，以及普通 IOI 假设下的 expected behavior。评估同时看 performance 和 faithfulness，避免“删掉一些组件仍能答对”被误称为原 circuit 解释完整。

## Experiments / Findings

base IOI circuit 在 DoubleIO/TripleIO 上仍保持很高性能，且整体复用原有 nodes、edges 和 mechanisms，只需增加额外 input edges。更意外的是，在原 IOI algorithm 理应失败的 prompt variant 上模型仍可输出正确答案；作者发现 S2 Hacking：电路利用第二个重复/副作用结构绕开原算法的假设，造成 apparent generalization。

## Ablation / Error Analysis

只看准确率会漏掉 S2 Hacking，必须同时检查 circuit faithfulness 和组件路径。移除新增 edges 会破坏 variant 上的补偿机制；只保留原 nodes 可能仍有 residual shortcuts。IOI circuit 的“泛化”因此不是一个固定程序简单复制，而是同一组件在不同输入结构下自适应地计算/利用额外边。

## Limitations

实验集中 GPT-2 Small 和 IOI family，不能代表自然语言任务的所有 prompt generalization；S2 Hacking 也可能是特定 prompt construction 的 artifact。circuit discovery 的节点粒度和阈值会影响“复用全部组件”的结论。

## 两句话总结

论文显示 GPT-2 的 IOI circuit 能跨 DoubleIO/TripleIO 复用原组件，并通过新增输入边适应不同 prompt。它发现的 S2 Hacking 提醒我们，任务泛化不等于原始算法 faithfully generalize，必须用 faithfulness 与反事实消融检查电路行为。

## Evidence note

本笔记逐段核对 arXiv PDF 的 prompt variants、Tables 1-2、circuit discovery、faithfulness 与 S2 Hacking analysis。

