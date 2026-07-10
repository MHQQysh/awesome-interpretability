# 073. Layered Bias: Interpreting Bias in Pretrained Large Language Models

- **作者 / venue**：BlackboxNLP 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.blackboxnlp-1.22/)

## Introduction / Method

LLM bias 不是一个单一分数，可能来自 embedding、attention、MLP、layer 和生成解码。论文用 open-ended prompts、gender/age/disability/sexual-orientation 等维度分析 bias，并将影响分解到 Transformer 层与 attention block。

## Baseline 与对比

比较 OPT、LLaMA 等模型、不同 bias metric 和 debiasing 方法；以 gender-pair direction、PANDA 数据集、LoRA/weight editing 等设置观察 bias score 与 downstream task 的变化。

## Findings

不同 bias metric 对同一个 debiasing method 的判断可能不一致；分数下降不一定代表真正无偏。bias 可能集中在特定层或模块，编辑后也可能转移到其他行为。论文强调要同时看 bias、任务性能和模型内部变化。

## 局限

开放式 generation 对 prompt 很敏感，bias score 50 也不能解释为“无偏”。社会偏见评测需要多群体、交叉身份和人工审计。

**一句话评价**：论文将 bias 从单一输出现象拆成 layered mechanism，并提醒 debiasing 结果必须跨指标核验。
