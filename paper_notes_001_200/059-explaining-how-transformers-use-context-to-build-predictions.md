# 059. Explaining How Transformers Use Context to Build Predictions

- **作者 / venue**：ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-long.301/)

## Introduction 与方法

Transformer 根据上下文生成下一个 token，但 attention map 或输入 attribution 不足以解释各层如何把上下文信息写入 prediction。论文分解 attention 与 MLP 对 logits 的贡献，研究 token-to-token context mixing、logit updates 和语法/语义证据的层级路线。

## Baseline 与对比

与传统 input attribution、attention visualization 和 model-agnostic explanation 对照；在 IOI 等任务和不同规模模型上比较 layer-wise contribution。论文还用随机/对照 token 以及替换 context 检查 logit update 是否真正依赖证据。

## Findings

MLP 和 attention 在预测中承担不同角色：attention 将相关上下文信息搬运到当前位置，MLP 对这些信息做 feature transformation 并写入 vocabulary-relevant directions。某些层会产生较大的 logit updates，且在 larger model 中仍可观察到类似对齐结构。

## 局限

logit decomposition 依赖模型参数和基底选择；局部组件贡献不等于完整 circuit。需要 activation patching 和 path-level controls 才能把相关性提升为因果解释。

**一句话评价**：论文把“模型看了哪些上下文”推进到“attention/MLP 如何把上下文变成 logits”的计算分解。
