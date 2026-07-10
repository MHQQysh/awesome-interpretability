# 090. Back Attention: Understanding and Enhancing Multi-Hop Reasoning in Large Language Models

> 人工精读笔记：本文先用 logit flow 定位 latent multi-hop reasoning 的失败阶段，再用 back attention 让低层重新访问高层 hidden state。

## 0. 论文信息

- **作者**：Zeping Yu, Yonatan Belinkov, Sophia Ananiadou
- **主题**：mechanistic interpretability、latent multi-hop reasoning、attention intervention
- **评测**：single-hop/two-hop synthetic tasks、GSM8K、SVAMP、TwoHop 等；5 个预训练 LLM

## 1. Introduction：要解决什么问题

LLM 有时能回答需要两次知识检索的问题，却不显式生成中间 chain-of-thought。已有分析发现，第一跳事实和第二跳事实可能都被模型存储，但关系信息没有可靠地从第一跳实体传给下一阶段，导致 latent multi-hop accuracy 很低。直接让模型生成 CoT 可以改善结果，却需要额外 token 和显著 inference cost。

作者先问：单跳/两跳预测中 logits 在不同位置和层如何流动？再问：能否只修改 attention 连接，让低层访问高层已形成的中间 feature，从而修复关系属性提取而不生成长 CoT？

## 2. Method / Framework：怎么解决

### 2.1 Logit flow

作者把 hidden state 经 unembedding 后的 logits 沿 layer/token 追踪，识别单跳知识预测的四个阶段：entity subject enrichment、entity attribute extraction、relation subject enrichment、relation attribute extraction。对两跳问题，错误通常集中在最后的 relation attribute extraction：模型同时保留互相竞争的 logits，最终选错第二跳实体。

### 2.2 Back attention

标准 transformer 中低层只能使用当前位置及之前层的计算结果，不能回头读取同一 token 的高层表示。back attention 在指定 layer 加一个额外 attention，使较低层的 query/key/value 重新访问更高层 hidden state，再把恢复的高层 feature 加回正常 forward。

作者在第 6 层等位置加入 back attention，并用可学习 attention score 决定哪些早期位置需要高层信息。训练时可从零训练，也可在预训练 LLM 上只 fine-tune back-attention 参数。

### 2.3 成本

若普通 CoT 生成 K 个 token，成本约 K·T；back attention 每 token 的重算约 1.8T，但只需生成 M 个 token，且 M 远小于 K，估计总成本约 1.8M·T。因此它以少量层内重算换取少量输出 token。

## 3. Baseline / 对比基线

- **原始 1-layer transformer**：检验 back attention 是否弥补层数不足。
- **2-layer transformer**：结构能力参照；论文报告 1-layer + back attention 可达到 2-layer accuracy。
- **原始预训练 LLM zero-shot**：评估 back attention 加入前后的真实收益。
- **CoT**：能力/计算对照，检验不用显式中间 token 是否也能恢复推理。
- **不同插入 layer**：在各层加入 back attention，寻找恢复信息的最佳位置。
- **五个 LLM、五个 reasoning dataset**：避免只在单一模型/任务中解释机制。

## 4. Experiments / Findings：结果如何读

### 4.1 从机制到干预位置

logit flow 显示两跳失败不是第一跳知识不存在，而是 relation attribute extraction 阶段的 conflicting logits 没有被正确消解。把 back attention 加到第 6 层时，隐藏状态重新获得与第一跳实体有关的重要 feature，错误竞争下降。

在从零训练的 1-layer transformer 上，原始模型准确率约 67.1%；在第 6 层加入 back attention 后峰值约 93.2%。1-layer + back attention 的收敛更快、参数量约为 2-layer 方案的 56.7%，并达到接近 2-layer 的能力。

### 4.2 预训练 LLM

在 Llama 3、Llama 3.2-3B、Mistral-7B、Qwen2.5 等五个模型上，back attention 在 GSM8K、SVAMP、TwoHop 等五个数据集上都提升准确率；表 1 报告多处达到大幅提升，部分任务接近翻倍，最高增益超过 70%。这说明干预并不是只修复合成 two-hop。

### 4.3 机制可视化

back-attention score 的可视化显示它主要从较早 token/层读取与第一跳相关的信息，再写回后续 relation processing。这个路径与 logit-flow 预先定位的 failure stage 对齐，因此方法不是纯黑盒 prompt trick，而是由内部分析驱动的 targeted intervention。

## 5. Ablation / 机制解释

- **插入层 sweep**：不同 layer 的 back attention 效果差异大，第 6 层附近在 toy transformer 上最佳，证明位置选择重要。
- **1-layer vs 2-layer**：隔离“更多层数”与“反向访问高层”的作用。
- **CoT vs back attention**：比较显式中间语言 token 与隐式 hidden-state route。
- **logit flow + score visualization**：先定位 failure，再确认 intervention 实际访问了相关 feature。
- **训练/预训练两种设置**：检验从零学习与对现有 LLM 的轻量适配是否一致。

## 6. Limitations / 局限与复现注意

- 主要分析单跳和两跳知识查询，复杂规划、长链数学和工具调用可能具有不同机制。
- 预训练 LLM 上的 back attention 需要 fine-tuning，部署时还要实现高层 hidden state 的缓存/重算。
- 当前只在一个 layer 上插入，多个 back-attention layer 的稳定性和副作用尚未充分研究。
- 计算成本低于长 CoT 的估计依赖 M 远小于 K；若任务仍需要大量输出 token，优势会缩小。

## 7. 两句话总结

论文用 logit flow 发现 latent multi-hop reasoning 的主要瓶颈是最后的 relation attribute extraction 阶段，第一跳形成的关键 feature 没有被低层继续访问。它提出 back attention 让低层回读高层 hidden state，在 toy transformer 上使 1-layer 接近 2-layer，并在五个 LLM/五个数据集上提升推理准确率，但当前证据仍集中在有限的 multi-hop 场景。
