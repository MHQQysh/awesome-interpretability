# 188. Journey to the Center of the Knowledge Neurons

- **Authors:** Yuheng Chen, Pengfei Cao, Yubo Chen, Kang Liu, Jun Zhao
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/29735
- **Tags:** Knowledge Neurons, Multilingual, Knowledge Editing

## Introduction

knowledge neurons 可能承载事实，但多语模型中不同语言是否共享、为什么同一事实有多个 neuron 尚不清楚。作者提出 architecture-adapted multilingual integrated gradients，研究 language-independent knowledge neurons（LIKN）与 degenerate knowledge neurons（DKN）。

## Method / Framework

在不同语言 query 上做 integrated gradients，定位事实相关 neuron；跨语言取交集得到 LIKN。对同一事实的多个 neuron 集合做过滤和功能测试，识别不同 neurons 共同存储同一事实的 degenerate overlap。

## Baselines / Comparisons

与已有 knowledge neuron localization、单语 integrated gradients 和不同 architecture / language 进行比较。指标包括 localization precision、cross-lingual editing success、fact recall、neuron overlap 和 editing side effects。

## Experiments / Findings

- LIKN 在英语、中文等语言中共同参与事实预测，跨语言 edit 可以借助共享 neuron 传播。
- DKN 表明多个不同 neuron 可能存储同一事实，functional overlap 解释了事实记忆的冗余和鲁棒性。
- architecture-adapted attribution 比直接套用旧方法更能处理不同 PLM 架构与语言。

## Ablation / Error Analysis

消融语言、IG baseline、neuron intersection / filter、事实类型和 edit layer。多义事实、tokenization、语言不对齐和共享神经元会造成误定位；只改一组 neuron 可能被其它 degenerate neurons 补回。

## Limitations

neuron localization 仍是 attribution evidence，不等于唯一存储位置；跨语言数据和模型有限。edit 后的泛化与副作用需要更长时间、更多语言测试。

## 两句话总结

论文发现多语 PLM 中存在跨语言共享的 LIKN，也存在多个 neuron 重复承载同一事实的 DKN。它解释了跨语言知识编辑和记忆鲁棒性，但定位与因果存储仍不能完全等同。
