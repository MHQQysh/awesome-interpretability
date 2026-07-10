# 306. Efficient Automated Circuit Discovery in Transformers Using Contextual Decomposition

- **Authors:** Aliyah R. Hsu, Georgia Zhou, Yeshwanth Cherapanamjeri, Yaxuan Huang, Anobel Y. Odisho, Peter R. Carroll, Bin Yu
- **Venue / year:** ICLR 2025
- **Paper:** https://arxiv.org/abs/2407.00886
- **Tags:** Automated Circuit Discovery, Contextual Decomposition, CD-T

## Introduction

自动 circuit discovery 通过 activation patching 或近似梯度找任务子图，但运行慢、对 non-zero gradients/metric 有要求，难以扩展到大模型和非线性路径。CD-T 的问题是能否用 contextual decomposition 高效分解 transformer 的 output contribution，直接构造 interpretable circuit。

## Method / Framework

Contextual Decomposition for Transformers (CD-T) 把每层 residual/attention/MLP computation 分成与目标 context/feature 相关的 beta 与背景 alpha contribution，并递归传播 through nonlinearities、normalization 和 attention。通过贡献分数选择 nodes/edges，形成 task-specific circuit；方法不需要对每个组件反复运行完整 clean/corrupted patch，也降低了 gradient metric 限制。

## Baselines / Comparisons

比较 activation patching、attribution patching、gradient/edge attribution 和其他 automated circuit discovery，评价 runtime、circuit sparsity、faithfulness、task logit recovery 与组件 ranking。小型 algorithmic tasks 和 transformer language tasks 用于检查 CD-T 是否找到已知/合理 circuit。

## Experiments / Findings

CD-T 能在更低计算成本下定位 task-relevant components，提供比逐组件 patch 更可扩展的 circuit discovery；它保留 context-specific contribution，同时输出可解释的 edge/node ranking。论文报告在多个 transformer tasks 上能恢复重要 circuit，显示 contextual decomposition 是自动化 MI 的实用替代路径。

## Ablation / Error Analysis

alpha/beta decomposition、nonlinearity approximation、threshold 和 circuit size 是关键消融；只看 contribution magnitude 可能把协同/抑制路径遗漏，近似误差在 LayerNorm/softmax/MLP nonlinearities 处累积。CD-T 需要用 activation patching 或 resample ablation 做 independent causal validation，不能因计算快就把 attribution 当充分证明。

## Limitations

contextual decomposition 仍是近似，复杂 attention interactions、path cancellation 和 feature superposition 会影响排名；大模型 activation capture 与 graph search 仍有成本。方法更多解决定位效率，不自动解决自然语言 feature 的语义命名。

## 两句话总结

CD-T 将 contextual decomposition 扩展到 transformer，用 alpha/beta contribution 高效构造 task-specific circuit，降低了自动 circuit discovery 的运行与梯度要求。它适合扩大 MI 工具规模，但近似贡献仍需要独立干预、负对照和路径完整性验证。

## Evidence note

本笔记逐段核对 ICLR/arXiv PDF 的 CD-T definition、automated circuit construction、baseline comparisons、runtime/faithfulness analysis 与 limitations。

