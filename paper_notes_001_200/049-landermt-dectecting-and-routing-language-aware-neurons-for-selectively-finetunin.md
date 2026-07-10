# 049. LANDeRMT: Detecting and Routing Language-Aware Neurons for Selectively Finetuning LLMs to Machine Translation

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.656/)
- **任务**：定位 language-aware FFN neurons，做选择性机器翻译 fine-tuning

## Introduction

对多语言 LLM 做全参数 MT fine-tuning 可能造成 catastrophic forgetting 和 parameter interference：提升一个语言对会损伤其他语言或任务。论文把问题转成 neuron routing：哪些 FFN layer/neuron 对 source-target language pair 最相关？能否只更新这些部分？

## Method

- 分析 LLM 的 FFN layers，测量 neuron 与 source/target language pair 的相关性。
- 路由 language-aware neurons，只对相关参数做 selective fine-tuning。
- 用少量 parallel data 训练翻译能力，尽量冻结共享/无关参数。
- 比较 language pair 之间的 neuron overlap 和干扰。

## Baseline 与对比

- 全参数 fine-tuning。
- 常规 multilingual MT fine-tuning。
- ICL/prompt-based translation。
- 不同 language pair 的 neuron selection 和 routing。

## Findings

- 选择性更新能改善目标翻译，同时减轻其他语言和原有能力的 forgetting/interference。
- 全参数 fine-tuning 不一定提高所有 language pairs，参数共享会把一个任务的更新传播到其他任务。
- language-aware neuron 分析提供了比“把所有 FFN 都训练一遍”更细的解释和控制单元。

## 局限

neuron relevance 对语言、数据量和层位置敏感；选择性更新的额外分析成本也需要计入。单个 neuron 不等于完整翻译 circuit，跨模型迁移仍需验证。

**一句话评价**：LANDeRMT 将多语言 fine-tuning 的干扰问题连接到 language-aware neuron routing，为可解释参数高效适配提供了路径。
