# 263. Enhancing Depression Detection with Chain-of-Thought Prompting: From Emotion to Reasoning Using Large Language Models

- **Authors:** Shiyu Teng, Jiaqing Liu, Rahul Kumar Jain, Shurong Chai, Ruibo Hou, Tomoko Tateyama, Lanfen Lin, Yen-Wei Chen
- **Venue / year:** IEEE EMBC 2025
- **Paper:** https://arxiv.org/abs/2502.05879
- **Tags:** Mental Health, Clinical NLP, Chain-of-Thought, Depression Detection

## Introduction

文本抑郁检测需要识别含蓄症状、情绪和潜在原因，直接把文本映射到 depression/severity label 容易遗漏临床线索，也没有透明的诊断路径。论文在 E-DAIC 上研究一个结构化 CoT prompt，目标不是让模型自由暴露长思维，而是按临床风格把诊断拆成可检查的四阶段。

## Method / Framework

prompt pipeline 依次要求模型做 sentiment/emotion analysis、binary depression classification、underlying-cause identification 和 severity assessment。这样把“情绪线索”和“诊断标签”分开，再让原因与严重度成为解释证据。实验只使用文本 modality，测试多个通用和原生 CoT LLM；指标为 Concordance Correlation Coefficient (CCC) 与 Mean Absolute Error (MAE)，分别衡量严重度估计的一致性和误差。

## Baselines / Comparisons

传统 multimodal/deep baselines 包括 AFT、Teng et al.、Tensorformer、STMCAT、CubeMLP、MIMRL；LLM baseline 包括 DeepSeek V3、Qwen2.5-Max、GPT-4o，以及原生 CoT 的 GPT o3-mini、GPT o1-preview、QwQ-32B-preview、DeepSeek R1。消融直接比较同一 LLM 有无该 CoT prompt。

## Experiments / Findings

- 传统方法中 CubeMLP CCC 0.583/MAE 4.37，MIMRL 0.580/4.36；标准 LLM 中 DeepSeek V3 0.622/4.15、Qwen2.5-Max 0.637/4.07、GPT-4o 0.732/3.37。
- 原生 CoT 模型中 DeepSeek R1 达到 CCC 0.708、MAE 3.49；作者强调结构化 prompt 同时改善准确率和 diagnostic granularity，而非只增加文本长度。
- 对 Qwen2.5-Max，加 prompt 后 CCC 从 0.550 提到 0.637，MAE 从 4.33 降到 4.07；GPT-4o 从 0.696/3.47 提升到 0.732/3.37。GPT o3-mini 从 0.625/3.84 提到 0.677/3.68，QwQ-32B-preview 从 0.597/4.23 提到 0.705/3.55。

## Ablation / Error Analysis

最直接的 ablation 是四阶段 prompt 的整体增益，但没有充分隔离单独 emotion、cause 或 severity 阶段的贡献。文本-only 造成对语音、表情和行为线索的遗漏；LLM 生成的“临床解释”也可能是 plausible post-hoc rationale，不应直接当作医疗诊断依据。CCC 提高不代表所有亚组公平，也不代表模型理解了病因。

## Limitations

论文只验证 E-DAIC 文本，计划未来加入 audio/visual；高风险心理健康场景还需要临床专家、隐私保护、校准与外部验证。CoT 的可读步骤提高可审查性，但并不等于 faithful internal reasoning，且 prompt 效果可能随模型版本改变。

## 两句话总结

该方法把抑郁检测拆成情绪、二分类、原因和严重度四个可检查阶段，用结构化 CoT 同时提升 E-DAIC 上的 CCC/MAE 与诊断细节。它证明 prompt 级流程能给普通 LLM 加上更清晰的推理 scaffold，但文本-only、临床风险和解释忠实性仍限制应用。

## Evidence note

本笔记逐段核对 IEEE/arXiv PDF 的 Introduction、Table I-III、baseline comparison、ablation 与 Conclusion；数值来自正文表格。

