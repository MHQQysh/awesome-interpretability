# 077. Extracting Latent Steering Vectors from Pretrained Language Models

- **作者 / venue**：Findings of ACL 2022
- **论文**：[ACL Anthology](https://aclanthology.org/2022.findings-acl.48/)

## Introduction / Method

可控生成通常依赖 trainable decoder、prompt 或 fine-tuning。论文从 frozen pretrained LM 的 hidden states 中直接提取 fixed-length steering vectors，把目标属性（长度、风格/类别等）转成 latent direction，并在 forward pass 中注入。

## Baseline 与对比

比较 autoencoder-based latent representation、mean-pooled GPT2/BERT hidden states、GloVe、prompt-based steering 和 trainable methods。还测试只用约 10 个 examples 时是否仍有效。

## Findings

提取的 steering vectors 能近乎完美恢复部分英文句子属性，并在 few-example 条件下保持竞争力。直接注入 transformer stack 的 steering 比只把向量放进 prompt 更稳定；这表明控制方向在 hidden representation 中，而非只在文字提示中。

## 局限

vector 可能混入 topic、length 和 style；不同层/模型的方向不可直接复用。steering 强度过大可能降低 fluency，且不代表模型内部概念是单一线性方向。

**一句话评价**：论文是 representation engineering 的早期基础，展示 frozen LM 也可以通过 latent direction 被控制。
