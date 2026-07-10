# 358. Partial Discharge Classification with Transformer Neural Networks

- **Authors:** Thepjit Cheypoca, Wiboon Promphanich, Aung Ye Thway, Angelina Phimpissada Hankae, Siwakorn Jeenmuang, Norasage Pattanadech
- **Venue / year:** 2024 IEEE ICPADM
- **Paper:** https://doi.org/10.1109/ICPADM61663.2024.10750584
- **Tags:** `transformer` `partial-discharge` `industrial-diagnosis` `interpretability-adjacent`

## Introduction
Partial-discharge pattern classification is used to identify insulation faults in high-voltage equipment. The paper's title and available conference metadata indicate a Transformer Neural Network approach for distinguishing discharge patterns, which is relevant to the broader interpretability list because the classifier is intended to support a physical diagnosis rather than only a generic label.

## Method / Framework
The accessible metadata describes phase-encoded partial-discharge signals, positional encoding, and a Transformer encoder, with a train/test split reported as 80%/20% in the available summary material. The target categories are described as corona, internal, and surface discharge. Because the publisher full text was not available in the working corpus, the exact input tensor construction, attention configuration, preprocessing, and training loss cannot be verified here.

## Baselines / Comparisons
The accessible records do not expose a reliable full baseline table. Search results contain a conflicting abstract that appears to describe a different probabilistic-neural-network/PCA paper, so that material is not treated as evidence for this Transformer paper. No numerical comparison is recorded rather than importing results from the similarly named 1D-CNN paper or older PNN work.

## Experiments / Findings
The available conference metadata indicates that the study varies network/neuron configuration and evaluates classification of the three discharge types. This establishes the paper's experimental intent, but not its exact accuracy, F1, confusion matrix, or best architecture. Those numbers require the IEEE proceedings PDF or an author-provided copy.

## Ablation / Error Analysis
The only verifiable design comparison is the reported investigation of neuron numbers and architecture choices. It is not possible to reconstruct which component was changed or how errors distribute across corona, internal, and surface classes from the accessible abstract-level evidence.

## Limitations
This note is intentionally evidence-bounded: the full paper was not accessible, and public index pages provide inconsistent abstract text. It should not be used as a substitute for the four-page IEEE paper. The missing evidence includes baselines, exact signal representation, hyperparameters, class balance, metrics, and error analysis.

## 两句话总结
可核实的信息表明，这篇 ICPADM 2024 论文用 Transformer 处理相位编码的局部放电脉冲模式，目标是区分 corona、internal 和 surface discharge。由于全文无法获得且公开摘要存在疑似错配，本文档只保留已确认的题目、任务和方法线索，不虚构准确率、baseline 或 ablation 结论。

## Evidence note
证据边界：当前只有 DOI、会议目录/索引和摘要级记录，未获得出版商全文；ResearchGate 返回的 PNN/PCA 摘要与题目不一致，因此已明确排除。
