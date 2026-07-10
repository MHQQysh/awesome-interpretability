# 072. Backpack Language Models

- **作者 / venue**：ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-long.506/)

## Introduction / Method

Backpack 将每个词表示为多个 non-contextual sense vectors，再由上下文生成非负组合权重。相比 Transformer 的 monolithic representation，sense vector 提供了可命名、可调节的解释和控制接口。

## Baseline 与对比

比较 GPT-2 size 的 Backpack LM 与普通 Transformer/word embedding；实验还比较 k=1 与 k>1 sense vectors，检查多 sense 是否必要。

## Findings

Backpack 在保持语言建模能力的同时，把词义拆成多个向量；k>1 对表达能力和性能很重要。sense vectors 可用于控制 stereotype，例如 CEO/nurse 的 gender association，通过缩放相关 sense 改变生成偏好。

## 局限

多个 sense 向量不自动等于人类词义；contextualization network 仍可能把任意复杂函数编码进去。控制一个 sense 可能造成语义或流畅度副作用。

**一句话评价**：Backpack 为语言模型提供了显式 sense-level 表示，展示了“可解释结构”和语言建模性能可以共同设计。
