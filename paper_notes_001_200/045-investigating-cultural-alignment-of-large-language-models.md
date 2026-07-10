# 045. Investigating Cultural Alignment of Large Language Models

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.671/)
- **任务**：评估 LLM 是否反映特定国家/文化群体的 survey response 分布

## Introduction

LLM 常被当作“集体知识”容器，但文化不是一个国家的单一标签。模型可能因为英语主导训练、prompt 语言、persona 设定而偏向某种文化视角。论文把 cultural alignment 定义为模型 response 与真实人群 survey distribution 的接近程度。

## Method 与指标

- 使用 Egypt 与 United States survey，比较不同 culture、persona 和 topic。
- 让模型回答与文化态度相关的问题，再与真实 survey response 比较。
- 设计 Hard metric（直接 response agreement）和 Soft metric（考虑 ordinal option order）。
- 使用 anthropological prompting，让模型以 anthropologist 方式考虑文化差异，而不是机械输出单一平均答案。

## Baseline 与对比

- 不同 prompt language：英语、阿拉伯语等。
- GPT-3.5、AceGPT-Chat 等模型和不同规模/语言训练背景。
- generic prompt vs anthropological prompt。
- culture persona 与个人 diversity 的对照，避免把模型对国家平均值的拟合误认为理解个体。

## Findings

- prompt language 会影响 cultural alignment；在 Egypt survey 上，用 Arabic prompt 的模型 alignment 可高于 English prompt。
- 文化对齐不是简单的模型语言能力问题，也受 persona、题目、人口分布和模型训练语料影响。
- anthropological prompting 能提高部分 alignment，但不应被解释为模型真正拥有文化经验。
- 论文强调群体内部差异：一个模型拟合国家平均 survey，并不代表它能生成多样、真实的个体回答。

## 局限

survey 数据受样本和问题设计限制；二国实验不能代表全球文化；Hard/Soft metric 仍是代理指标。文化 alignment 的提升也可能来自 prompt-induced stereotype。

**一句话评价**：论文把文化偏差从定性案例变成 survey-level alignment 评测，并证明 prompt 语言和“文化专家式”提示会改变结果。
