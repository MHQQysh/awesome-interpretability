# 076. Knowledge Neurons in Pretrained Transformers

- **作者 / venue**：ACL 2022
- **论文**：[ACL Anthology](https://aclanthology.org/2022.acl-long.581/)

## Introduction / Method

预训练 Transformer 能用 cloze 任务回忆事实，但事实存在哪里不清楚。论文用 knowledge attribution 在 layer/neuron activation 上定位表达某个 relational fact 的 neurons，并观察 suppress/amplify 对事实预测的影响。

## Baseline 与对比

比较 knowledge neurons、同层随机 neurons、同关系/跨关系 fact pairs 和不同 attribution threshold。指标包含事实预测概率、PPL/准确率以及 activation editing 后的行为。

## Findings

knowledge neuron activation 与对应事实表达正相关；抑制这些 neurons 会降低事实预测，增强则提高，提供了比 probing 更接近因果的证据。神经元并不总是一对一存储事实，多个 neuron 和层共同参与，且知识可能存在冗余。

## 局限

cloze prompt 和 attribution score 会影响定位；“事实 neuron”不能直接解释完整生成过程。编辑可能影响相关事实和语义邻域。

**一句话评价**：这是知识神经元研究的代表工作，把参数记忆与可干预 neuron 联系起来，但仍需防止把相关 feature 过度命名为单条知识。
