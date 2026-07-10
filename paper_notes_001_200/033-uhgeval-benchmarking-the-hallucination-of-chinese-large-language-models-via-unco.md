# 033. UHGEval: Benchmarking the Hallucination of Chinese Large Language Models via Unconstrained Generation

- **作者 / venue**：Junjie Li, Xiaoxue Cheng, Wayne Xin Zhao, Jian-Yun Zhao, et al.；ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.288/)
- **任务**：中文 LLM unconstrained generation hallucination benchmark

## 1. Introduction

许多 hallucination benchmark 使用预设选项或固定方向构造样本，容易让模型学会评测模式，不能覆盖真实开放式生成。中文 LLM 的事实性评测还缺少统一、多领域、自由生成的测试协议。

## 2. Method / dataset

- 让多个中文 LLM 在开放约束下生成文本，模拟真实使用中的 hallucination。
- 使用五个中文 LLM 参与数据构造，包括 ChatGLM2-6B、Baichuan2-13B 等。
- 设计跨领域、跨任务的 prompt 和人工/自动标注流程，分析 hallucinated text 的类型、来源和严重程度。
- 与 TruthfulQA、HaluEval、HaDes 对比，说明 UHGEval 不只复用固定问题或预定义负例。

## 3. Baseline 与比较

- TruthfulQA：偏向事实知识与对抗性问题。
- HaluEval：大规模 hallucination 评价基线。
- HaDes：面向 hallucination detection 的数据集。
- 不同中文 LLM、领域和 prompt 约束，比较模型是否在开放生成中出现系统性错误。

## 4. Findings

- unconstrained generation 能产生比固定选项更丰富的 hallucination 类型，包括事实编造、时间/实体错误和上下文不一致。
- 不同中文模型在不同领域的 hallucination 分布并不一致，单一总分无法解释模型风险。
- 论文强调，模型看起来流畅并不代表对已有知识理解正确；训练语料覆盖、指令对齐和生成策略都会改变 hallucination。
- UHGEval 为中文模型提供了统一对照，也帮助区分“模型不知道”与“模型自信编造”。

## 5. 局限

开放生成标注成本高，自动评测器可能带来语言和知识偏差；中文领域知识更新快，数据污染和时间切片必须单独控制。

**一句话评价**：UHGEval 的关键价值是用 unconstrained generation 逼近真实使用，而不是让模型在预设答案选项中完成 hallucination 识别。
