# 271. Measuring the Interpretability and Explainability of Model Decisions of Five Large Language Models

- **Authors:** Kaito Fujiwara, Miyu Sasaki, Akira Nakamura, Natsumi Watanabe
- **Venue / year:** 2024 preprint
- **Paper:** https://osf.io/d4ntw/download
- **Tags:** LLM Evaluation, Interpretability, Explainability, Trust

## Introduction

“模型会解释”与“解释真的准确、相关且稳定”不是一回事。本文把 interpretability/explainability 当作可比较的模型属性，研究五个 LLM 在不同任务和输入下能否向人类说明决策过程，并关注 explanation accuracy、context relevance、一致性与用户 trust。

## Method / Framework

研究比较 TripoSR、Gemma-7B、Mistral-7B、Llama-2-7B 和 GemMoE-Beta-1，结合 qualitative inspection、quantitative benchmark、解释质量与图表分析。评价框架把 transparency/decision reasoning、explanation accuracy、contextual relevance 和跨输入 consistency 分开，而不是只用一个语言流畅度分数；还考察用户能否理解并信任模型输出。

## Baselines / Comparisons

五个模型本身构成主要比较，覆盖开源 dense、MoE 与多模态/生成式系统。论文没有提出新的 attribution/circuit 算法，因此 baseline 是不同模型在同一解释评价协议下的行为差异；文中还将结果与 XAI 中可解释、可审计、面向用户的标准联系起来。

## Experiments / Findings

公开摘要报告 TripoSR 与 GemMoE-Beta-1 在 transparency 上较好，Gemma-7B 与 Llama-2-7B 在 explanation accuracy 上较好，说明“解释透明”与“解释正确”不是同一维度。不同输入下的 consistency 与对反馈的适应性仍不稳定；模型的总体能力不能直接推断其解释能力。

## Ablation / Error Analysis

最重要的误差来源是模型能生成 plausible 叙述但未必说明真实 decision process，且不同任务/输入的解释尺度不一致。若只看 accuracy，会忽略 TripoSR/GemMoE 的透明度优势；若只看 transparency，又会忽略 Gemma/Llama 的事实性差异。论文倡导标准化 benchmark、反馈适应和多学科评价，但公开版本没有提供足够的组件消融。

## Limitations

模型选择、任务设计与主观评分会影响排序；摘要级公开信息不足以复核全部数据集、统计显著性和评分细节。解释 evaluation 仍是 proxy，不等于因果 faithfulness 或用户长期正确决策。

## 两句话总结

该研究用统一的质性和量化协议比较五个 LLM，发现 transparency、explanation accuracy 和 consistency 会给出不同排名。它说明“更大的模型”并不自动产生更好的解释，也提示解释性需要标准化、多维度和用户中心的评价。

## Evidence note

本笔记核对公开预印本摘要、Introduction 与结果讨论；因公开文本的实验表格不完整，未补写无法复核的数值。

