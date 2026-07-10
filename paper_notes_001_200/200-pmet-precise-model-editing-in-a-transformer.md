# 200. PMET: Precise Model Editing in a Transformer

- **Authors:** Xiaopeng Li, Shasha Li, Shezheng Song, Jing Yang, Jun Ma, Jie Yu
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/29818
- **Tags:** Model Editing, MHSA, FFN, Knowledge Localization, Locality

## Introduction

ROME 和 MEMIT 把 FFN 看作 key-value memory，能以较低成本编辑事实，但 Transformer layer 的 hidden state 并不只来自 FFN，还经过 MHSA 与 residual connection。忽略 MHSA 会把知识提取阶段和知识写入阶段混在一起，导致编辑对相关表达有效却不够精确；PMET 研究如何利用这条信息流改进 factual editing。

## Method / Framework

PMET 在 critical layer 同时优化 MHSA 与 FFN 两个分量的 transformed hidden states，从而区分“提取目标事实的 attention 状态”和“写入事实的 FFN 状态”；但最终只用优化后的 FFN 状态更新 FFN 权重，避免直接改写 MHSA 造成更大副作用。对于多层编辑，它用 square-root residual spread 把更新分配到 critical layers，而不是像 MEMIT 一样均匀分配。

## Baselines / Comparisons

在 zsRE 和 COUNTERFACT 上比较 Constrained Fine-Tuning (FT+W)、MEND、ROME、MEMIT。指标包括 efficacy、generalization，二者合称 reliability；specificity/locality 衡量无关知识保持，另外报告 editing score、fluency 和 consistency。实验包括 COUNTERFACT 的 10K mass edits，并在 GPT-J 6B、GPT-NeoX 20B 上测试。

## Experiments / Findings

- 在 COUNTERFACT 10K edits 上，PMET 在综合 score、efficacy、fluency 和 consistency 上优于现有方法；specificity 略低于 MEMIT，但明显体现 reliability 与 locality 的 trade-off。
- 与 MEND 相比，PMET 更偏向可靠地写入并泛化目标知识，而 MEND 的 specificity 更强；这说明模型编辑没有单一“全优”指标，必须同时看目标成功和非目标保持。
- zsRE 与 COUNTERFACT 的结果都支持 PMET 的核心假设：MHSA 包含一般性的知识提取模式，并可能存储少量事实相关信息；只更新 FFN 仍能利用这部分信息来生成更精确的写入目标。

## Ablation / Error Analysis

三组消融分别检验同时优化 MHSA/FFN、是否利用 MHSA 信息、以及 square-root residual spread。去掉 MHSA 优化会损失 reliability，错误的 layer selection 会把更新写入非关键位置，改回均匀 spread 会降低多层编辑精度。编辑冲突、关系泛化不足和相邻事实污染仍会造成 specificity 下降。

## Limitations

编辑评测主要是知识问答与 counterfactual benchmark，不能保证真实部署中的长期事实更新、对话一致性和安全性。PMET 仍依赖关键层假设与模板化知识线索；“MHSA 是提取器”是由干预实验支持的机制解释，但不等于所有事实都局部存储在固定组件。

## 两句话总结

PMET 识别到 Transformer hidden state 同时经过 MHSA、FFN 和 residual，因此优化两种组件的状态、只更新 FFN，并用平方根方式分配多层编辑残差。它在 zsRE 和 COUNTERFACT 上提升了编辑可靠性及大规模一致性，但 specificity trade-off 与 benchmark 外部效度仍需解决。
