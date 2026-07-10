# 285. An X-Ray Is Worth 15 Features: Sparse Autoencoders for Interpretable Radiology Report Generation

- **Authors:** Ahmed Abdulaal, Hugo Fry, Nina Montaña-Brown, Ayodeji Ijishakin, Jack Gao, Stephanie L. Hyland, Daniel C. Alexander, Daniel C. Castro
- **Venue / year:** 2024 preprint, under review
- **Paper:** https://arxiv.org/abs/2410.03334
- **Tags:** Vision-Language, Radiology, SAE, Interpretable Generation

## Introduction

VLM 自动生成 radiology report 面临 hallucination、解释性差和微调成本高。SAE-Rad 的问题是能否把冻结的 radiology vision encoder latent 拆成少量具有人类临床语义的 feature，再让 off-the-shelf LLM 根据这些 feature 生成报告，从而让视觉决策路径可检查。

## Method / Framework

SAE-Rad 在预训练 image encoder 的 class token/latent 上训练 hybrid SAE，平衡 reconstruction fidelity 与 sparsity；用现有 ground-truth reports 和 off-the-shelf language model 为每个 SAE feature distil radiological description。推理时图像先激活少量 features，再把 feature descriptions 编译成完整报告，不更新大型 vision/language backbone。辅助 indication、previous reports 等上下文也可注入。

## Baselines / Comparisons

在 MIMIC-CXR 上与 CheX-agent、MAIRA-2 等 radiology report generation systems 比较，另做 expansion factor 32/64/128、sparse/dense SAE、是否加入 indication/previous reports 的消融。指标包括 precision、recall、F1、clinical/radiology metrics 和一般语言 BLEU 等。

## Experiments / Findings

SAE-Rad 在不微调大模型的前提下达到有竞争力的 radiology-specific performance，接近更大/更昂贵的 MAIRA-2，并在 clinical feature 上优于 CheX-agent；一个配置的 Precision/Recall/F1 为 38.45/32.42/35.18，加入 indication 比无 indication 的 F1 32.67 更好。定性检查显示 feature 对 pathology presence/absence 有清晰语义，多个 feature 组合成较完整报告。

## Ablation / Error Analysis

expansion factor 太小会 feature absorption，太大/过 dense 会降低稀疏与可读性；报告中加入 indication、previous reports 能改变 precision-recall trade-off。Clinical accuracy 与 BLEU 不总一致：模型可能临床上抓对病变但文字不如人类流畅。冻结 encoder/LLM 的 bias 和 feature description LLM 的错误也会传入报告。

## Limitations

预印本、MIMIC-CXR 单一数据集和冻结 backbone 限制泛化；报告生成仍可能 hallucinate，SAE feature description 不等于医生确认。临床部署还需要多中心外部验证、病变定位、校准和专家审阅。

## 两句话总结

SAE-Rad 把 X-ray encoder latent 分解成约十几个可读 radiology features，再由 LLM 将其组合成报告，避免重新微调大型 VLM。它在临床指标上有竞争力并提供可追踪视觉证据，但语言流畅度、冻结模型偏差和临床验证仍是风险。

## Evidence note

本笔记逐段核对预印本 PDF 的 SAE-Rad pipeline、MIMIC-CXR results、Table 1-2、expansion/context ablations 与 conclusion。

