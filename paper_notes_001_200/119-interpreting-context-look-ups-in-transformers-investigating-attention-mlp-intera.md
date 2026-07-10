# 119. Interpreting Context Look-ups in Transformers: Investigating Attention-MLP Interactions

> 人工精读笔记：EMNLP 2024。重点是 attention head 如何触发 next-token MLP neuron，以及 context look-up 的组合机制。

## 0. 论文信息

- **作者**：论文作者团队
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.930)
- **模型**：GPT-2 Large、Llama-2 等；The Pile prompt samples

## 1. Introduction：要解决什么问题

Transformer 的 attention 常被描述为从上下文取信息，MLP 则把信息写进 vocabulary；但 attention-MLP 之间到底怎样协同触发 next-token neuron，仍缺少细粒度解释。只看 attention map 或单个 MLP neuron 都难以还原完整 look-up。

## 2. Method / Framework：怎么解决

先找强烈预测特定 next token 的 MLP neuron，再统计哪些 attention head 在其 max-activating prompts 中负责激活它。对这些 head 按 active/inactive prompt 分组，让 GPT-4 生成 head 功能解释，再在 held-out prompts 上测试解释能否预测 head 是否激活。

最后对 selected heads 做 ablation，观察目标 token probability 下降，验证 attention-MLP association 的因果作用。

## 3. Baseline / 对比基线

随机 attention heads、随机 late-layer neurons、activation skew、单独 neuron/head explanation、不同 GPT-4 auto-label prompt。比较 GPT-2 Large 与更小模型，检验发现是否依赖规模。

## 4. Experiments / Findings：结果如何读

一些 attention heads 只在特定 phrase/context 出现时激活对应 next-token neuron，例如时间范围、固定短语或词汇模式；GPT-4 根据 active/inactive examples 能生成可用于分类的新解释，结果通常高于随机 baseline。

head ablation 会降低关联 next-token 的概率，支持 head 真正在触发 neuron，而不是因为共同被输入激活。结果也显示多个 head 可以共同服务一个 neuron，一个 head 也可能对应多个 neuron，context look-up 是稀疏但非一对一的。

## 5. Ablation / 机制解释

随机 head/neuron、active/inactive prompt、GPT-4 explanation classifier、head zeroing 和不同模型规模构成消融。作者特别指出 activation skew 单独不足以解释功能，必须结合 held-out prediction 与 ablation。

## 6. Limitations / 局限与复现注意

The Pile 自然文本和 max-activating prompt 选择存在偏差；GPT-4 explanation quality 不是机制真值；单头 ablation 可能被冗余 head 补偿；结果主要描述 next-token phrase lookup。

## 7. 两句话总结

论文把 context look-up 解释成 attention heads 激活特定 next-token MLP neurons 的协同过程，并用 GPT-4 生成 head explanation、held-out 分类和 ablation 验证。结果支持稀疏但多对多的 attention-MLP 关系，同时提醒可读解释不能替代功能干预。
