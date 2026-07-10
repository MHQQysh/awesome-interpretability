# 482. A Survey on Large Language Models: Applications, Challenges, Limitations, and Practical Usage

- **Authors:** TechRxiv survey entry
- **Venue / year:** TechRxiv 2023
- **Paper:** [TechRxiv record](https://www.techrxiv.org/doi/pdf/10.36227/techrxiv.23589741.v1)
- **Tags:** `survey` `LLM-applications` `limitations` `practical-usage`

## Introduction
综述试图把 LLM 的 NLP、机器翻译、问答和应用部署放在同一张图中，并总结模型能力之外的 hallucination、bias、成本、数据和安全限制。对可解释性而言，它提供的是领域地图和风险清单，而不是新的 mechanistic method。

## Method / Framework
文章按模型基础、训练/微调、prompting、应用和现实部署挑战组织文献，并讨论如何将 LLM 接入实际任务。主要分析路线是能力、应用、局限和使用建议的 taxonomy，而非可复现的新模型实验。

## Baselines / Comparisons
survey 对照传统 NLP/专用模型、不同规模的 generative LLM、prompting/fine-tuning/RAG 等使用模式。由于公开版本主要是综述文本，不能把其中列举的应用结果当成统一实验设置下的 baseline。

## Experiments / Findings
综述的共同结论是：规模与 instruction tuning 提升通用性，但模型仍会 hallucinate、对 prompt 敏感、受训练数据偏差影响，且算力/隐私/部署成本限制 practical use。可解释性应和 factuality、uncertainty、human oversight 一起评估，而不能只看生成流畅度。

## Ablation / Error Analysis
没有统一 ablation；文章归纳的 failure 包括知识过时、事实错误、偏见、有害输出、不可解释的内部决策和 domain shift。不同 application 的 accuracy 不能直接横向比较，数据集与 prompting 细节是主要混淆因素。

## Limitations
TechRxiv survey 的文献筛选和 taxonomy 不能替代逐篇实验核验，且版本/覆盖范围会随领域快速变化。本文档保留证据边界，没有补写该版本未提供的表格数字。

## 两句话总结
这篇综述把 LLM 应用、训练方式、实用限制和风险做了宽范围整理，适合作为研究入口。对 interpretability 的启示是把解释与 hallucination、uncertainty、bias 和实际审计连起来，而不是把 fluent output 当成理解。

## Evidence note
当前可获得的是 TechRxiv 元数据/摘要级材料；没有把未能核对的完整实验数字写入本笔记。
