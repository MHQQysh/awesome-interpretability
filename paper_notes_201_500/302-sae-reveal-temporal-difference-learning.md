# 302. Sparse Autoencoders Reveal Temporal Difference Learning in Large Language Models

- **Authors:** Can Demircan, Tankred Saanum, Akshay K. Jagadish, Marcel Binz, Eric Schulz
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2410.01280
- **Tags:** In-Context Learning, Temporal Difference Learning, SAE, RL Reasoning

## Introduction

LLM 能在 prompt 中根据少量示例适应任务，但它是否能隐式执行 RL 算法很难从答案看出。论文研究 Llama 3 70B 是否能在上下文中解决简单 reinforcement learning tasks，并用 SAE 找到与 temporal-difference error、Q-value 对应的 representation。

## Method / Framework

作者设计三类 in-context RL tasks，让模型从示例中估计 state/action reward dynamics；随后对 residual stream 训练 SAE，在不同位置查找与 TD error、value/Q-value 的 activation correlation。最后对候选 SAE direction 做 activation intervention，改变其 magnitude 或 patch counterfactual state，测试 TD error/Q-value 计算和最终答案是否随理论预测变化。

## Baselines / Comparisons

比较 Llama 3 70B 的 task behavior、random/unrelated SAE features、不同层/位置 feature 和无干预 baseline；方法层面以行为 probe、相关分析和 causal intervention 互相校验。重点不是与 RL solver 的最终准确率竞争，而是证明 hidden feature 是否具有算法变量的功能。

## Experiments / Findings

Llama 3 70B 能在三类简单 RL task 中进行 in-context learning；SAE 发现的 latent 与 TD errors 和 Q-values 紧密对应，即使模型只接受 next-token prediction 训练。干预这些 direction 会改变模型对 TD error/Q-value 的计算和答案，支持 LLM 在上下文中复用类似 temporal-difference learning 的内部结构。

## Ablation / Error Analysis

跨 task、层和 SAE feature 的复现是关键；相关性 probe 可能只是数值/文本表面模式，因此 intervention 必须有 control direction 和 sign/scale 对照。简单 RL 环境可能让模型通过模式记忆解决，不能仅凭 feature 名称证明通用 RL agent；state aliasing 和 prompt order 也可能改变方向。

## Limitations

任务规模和模型家族有限，研究尚未定位完整 attention/MLP circuit；SAE feature 是算法变量的表示 proxy，不等于模型真的执行标准 RL update。复杂、多步、部分可观测环境的泛化仍未知。

## 两句话总结

论文在 Llama 3 70B 的 in-context RL 中用 SAE 找到与 TD error、Q-value 对应的 latent，并用干预证明它们参与答案计算。它为“LLM 是否隐式学会 RL 算法”提供了机制证据，但完整 circuit 与复杂环境泛化仍未解决。

## Evidence note

本笔记逐段核对预印本 PDF 的三类 RL tasks、SAE feature search、intervention protocol 与 findings；未把摘要之外的隐藏表格数字补入。

