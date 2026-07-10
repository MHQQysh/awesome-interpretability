# 198. T-SciQ: Teaching Multimodal Chain-of-Thought Reasoning via Large Language Model Signals for Science Question Answering

- **Authors:** Lei Wang, Yi Hu, Jiabang He, Xing Xu, Ning Liu, Hui Liu, Heng Tao Shen
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/29884
- **Tags:** Multimodal CoT, ScienceQA, LLM Teaching, Distillation

## Introduction

ScienceQA 需要语言、图像、常识和多步科学推理；人工 CoT 标注昂贵，而且标注解释可能缺少开放世界知识。论文研究如何让大 LLM 生成教学信号，再训练较小的 multimodal student，同时保留 planning、视觉证据和可检查的推理过程。

## Method / Framework

T-SciQ 用 zero-shot prompting 生成两类信号：QA-CoT 直接从问题到答案的 chain-of-thought，QA-PCoT 则先规划解题步骤再完成 CoT。作者混合两类信号形成 T-SciQ 数据集，训练 Multimodal-T-SciQ；学生同时接收文本和视觉特征，视觉侧比较 CLIP、DETR、ResNet，默认采用更适合对象级定位的 DETR 特征。

## Baselines / Comparisons

在 ScienceQA 上比较 human-annotated Multimodal-CoT、仅 QA-CoT、仅 QA-PCoT、语言-only student、LLaVA、Chameleon 等；进一步比较不同 teacher LLM、T-SciQ 数据比例和 CLIP/DETR/ResNet 特征。指标是 overall accuracy 以及 NAT/SOC/LAN、TXT/IMG/NO、年级等类别切分。

## Experiments / Findings

- Multimodal-T-SciQ-Large 达到 96.18% overall accuracy，超过 Multimodal-CoT-Large 的 91.68%（+4.50）、LLaVA 的 90.92%（+5.26）、Chameleon 的 86.54%（+9.64）和报告的人类 88.40%。
- Base student 的混合信号达到 91.75%，高于只用 QA-CoT 的 85.99%、只用 QA-PCoT 的 88.56% 和 Multimodal-CoTBase 的 84.91%；随着 T-SciQ 信号在训练集中的比例上升，性能整体上升。
- 视觉特征消融中，DETR 通常优于 CLIP/ResNet；案例显示 T-SciQ 能补充人工 CoT 缺失的地理常识，并帮助分解没有图像输入但需要多步推理的问题。

## Ablation / Error Analysis

核心消融是 QA-CoT、QA-PCoT 的单独/混合训练、生成数据比例、teacher LLM 和视觉特征。只使用一种信号会损失规划或执行能力；视觉特征不足会影响 image-context 题；LLM 教师错误、开放世界知识过时和合成 rationale 偏差会被 student 学到。

## Limitations

ScienceQA 的成绩不能直接代表通用多模态 reasoning；96.18% 依赖特定 teacher、提示和混合比例，人工 CoT 与合成 CoT 的真实性也未做因果验证。大模型生成教学数据仍有 API/计算成本，学生可能学到答案模式而不是可迁移的科学推理。

## 两句话总结

T-SciQ 用大 LLM 生成 QA-CoT 与 plan-based QA-PCoT，再混合教学小型多模态模型，解决人工 CoT 昂贵和知识覆盖不足的问题。它在 ScienceQA 上显著超过人工 CoT 和多种多模态 baseline，但提升依赖合成信号质量、视觉特征和任务分布。
