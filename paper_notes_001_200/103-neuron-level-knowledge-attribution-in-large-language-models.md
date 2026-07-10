# 103. Neuron-Level Knowledge Attribution in Large Language Models

> 人工精读笔记：EMNLP 2024。重点是区分 value neurons 与 query neurons，并用 log-probability increase 作为更稳定的 neuron importance。

## 0. 论文信息

- **作者**：Zeping Yu, Sophia Ananiadou
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.191)
- **模型/数据**：GPT-2-large、Llama-7B；language、capital、country、color、number、month 等 factual relations

## 1. Introduction：要解决什么问题

已有 knowledge neuron 工作通过激活或梯度找到与事实相关的 FFN neuron，但 neuron attribution 常受 saliency 选择、token 位置和输出层影响。论文希望从 vocabulary/logit space 直接问：某个 neuron 的干预是否真的提高正确知识 token 的概率？哪些 neuron 是存储 value，哪些 neuron 是把 query 传给最终预测的？

## 2. Method / Framework：怎么解决

### 2.1 Value neurons

分析 FFN/attention neuron 对最终 token log probability 的贡献。相比只用 activation magnitude，作者把 neuron 激活前后对正确答案 log probability 的提升作为 importance score，避免一个 neuron 仅仅活跃但不支持目标 token。

### 2.2 Query neurons

对 top value neurons 追踪哪些前层 residual/FFN neurons 通过 attention 或 MLP 激活它们。这样形成 shallow/medium query -> deep value -> final prediction 的知识流，而不是把所有贡献都归到最后一层。

### 2.3 Intervention

干预 top neurons，比较正确答案 MRR/probability 下降；再与同数量随机 neuron intervention 对照。若少量 top neuron 造成大幅下降，则支持它们在知识 attribution 中具有功能重要性。

## 3. Baseline / 对比基线

- activation magnitude、gradient/saliency 等常规 neuron score。
- probability increase 与 score variants 的对照。
- layer/head/neuron-level attribution，比较定位粒度。
- top-100/top-200 与随机同数量干预，检验功能性。
- GPT-2 与 Llama replication，避免架构特例。

## 4. Experiments / Findings：结果如何读

log-probability increase 比单纯 activation magnitude 更能定位重要 neurons。GPT-2 的重要 attention/value neurons 常位于中深层，Llama 也显示 medium/deep layers 对最终 factual token 贡献大；但 query neurons 往往来自更浅或中层，负责把输入实体关系传入 value circuitry。

干预 top-200 attention neurons 和 top-100 FFN neurons 会显著降低正确答案 MRR/概率，而随机干预同数量 neuron 影响明显小得多。实验还发现，只有少量 neurons 并不能解释所有知识：许多 neuron 协同贡献，300 个左右的干预已经足以改变最终预测。

不同 relation 的 top heads/neurons 有重叠，说明模型可能复用类似知识存储机制；同时有 relation-specific neurons，说明统一的“一个知识一个 neuron”叙述过于简单。

## 5. Ablation / 机制解释

- score 由 activation/gradient 改为 log-probability increase，验证 output-aware attribution。
- layer/head/neuron 三个层级互相对照。
- top neurons vs random neurons 干预，检验相关性是否有 causal support。
- value neurons 与 query neurons 分离，解释信息从输入到最终答案的流动。

## 6. Limitations / 局限与复现注意

- factual cloze queries 只覆盖有限关系，不能代表复杂生成或多跳 reasoning。
- neuron importance 依赖目标 token、prompt template 和 intervention scale。
- top neuron 干预会有协同和冗余，单 neuron 语义不能直接被解释为完整知识。
- 结果主要在 GPT-2/Llama，模型编辑、SAE feature 和更大模型上需要重新验证。

## 7. 两句话总结

论文提出以正确答案 log-probability increase 衡量 neuron 对事实知识的贡献，并进一步区分负责存储答案的 value neurons 与负责传递 query 的 query neurons。跨 GPT-2 和 Llama 的干预表明 top neurons 比随机 neurons 更能改变事实预测，但知识仍由多层、多 neuron 协同实现。
