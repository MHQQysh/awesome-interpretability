# 212. LayoutLLM: Layout Instruction Tuning with Large Language Models for Document Understanding

- **Authors:** Chuwei Luo, Yufan Shen, Zhaoqing Zhu, Qi Zheng, Zhi Yu, Cong Yao
- **Venue:** CVPR 2024
- **Paper:** https://doi.org/10.1109/CVPR52733.2024.01480
- **Tags:** Document Understanding, Layout, LayoutCoT, Instruction Tuning

## Introduction

文档问答和信息抽取不仅需要 OCR 文字，还需要知道文字在页面上的区域、顺序和几何关系；把 layout 直接序列化成坐标文本并不稳定。LayoutLLM 研究如何让 7B LLM/MLLM 在预训练与指令微调中真正利用 document layout，并让回答过程可以被人工检查。

## Method / Framework

Layout instruction tuning 包含 layout-aware pre-training 与 layout-aware SFT。预训练覆盖 document-level dense caption/text-layout reconstruction、region-level layout analysis、segment-level table understanding 等；SFT 引入 LayoutCoT，把回答分为 question analysis、relevant area concentration、answer formation，显式提示模型先找相关区域再生成答案。

## Baselines / Comparisons

在 DocVQA、VisualMRC、FUNSD、CORD、SROIE 上比较 Llama2/Vicuna 的 plain text 与 layout text、Qwen-VL、mPLUG-DocOWL、G-DocOWL，以及 LayoutLMv3 等 document-pretrained model。指标包括 DocVQA/VisualMRC accuracy 与 VIE 的 F1/ANLS，并比较 Llama2-7B-chat 与 Vicuna-1.5-7B backbone。

## Experiments / Findings

- 零样本文档结果中，LayoutLLM-7B 达到 DocVQA 74.25/74.27、FUNSD 78.65/79.98 等，明显超过同类开源 7B LLM/MLLM；LayoutCoT 让答案带有可检查的相关区域推理。
- 消融 baseline（无 layout pre-training/SFT）在 DocVQA/FUNSD 为 70.82/70.96；加入 layout-aware pre-training 后为 72.31/74.02，再加入 layout-aware SFT/CoT 达到 74.27/79.98。
- pre-training 对基础文档理解有稳定收益，LayoutCoT 对复杂 key-value extraction 和区域相关任务提升更明显；但模型仍难精确处理 region-level relationships。

## Ablation / Error Analysis

消融 layout pre-training、layout-aware SFT、LayoutCoT、plain/layout text 和不同 LLM backbone。只加坐标文本可能因序列噪声导致不稳定，缺少 CoT 时模型会直接猜答案；区域切分、OCR 错误、表格跨行关系和密集页面是主要失败来源。

## Limitations

LayoutCoT 是提示式可读轨迹，不等于模型内部真正按所述区域完成决策；训练需要布局标注/合成任务和较大计算。论文明确指出 region-level relationship 仍弱，跨语言、手写、长文档和开放式多页推理尚未充分验证。

## 两句话总结

LayoutLLM 用 layout-aware pre-training 加 LayoutCoT，让 7B LLM 在文档问答前分析问题并集中到相关页面区域。它在 DocVQA/FUNSD 等任务上显著提升且更易人工检查，但坐标噪声、区域关系和解释轨迹的真实性仍是瓶颈。
