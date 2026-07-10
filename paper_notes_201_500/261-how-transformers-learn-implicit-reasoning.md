# 261. How Do Transformers Learn Implicit Reasoning?

- **Authors:** Jinhua Ye, Zijun Yao, Zhidian Huang, Liangming Pan, Jinxin Liu, Yushi Bai, Amy Xin, Weichuan Liu, Xiaoyin Che, Lei Hou, Juanzi Li
- **Venue / year:** NeurIPS 2025
- **Paper:** https://arxiv.org/abs/2505.23653
- **Tags:** Implicit Reasoning, Multi-Hop, Representation Geometry, Semantic Patching

## Introduction

显式 CoT 会把中间步骤写出来，但模型也可能在不输出中间实体的情况下完成 multi-hop query。问题在于，正确答案究竟来自记忆、shortcut 还是内部的组合推理，预训练模型上很难区分。论文在从头训练的可控 symbolic environment 中研究 (e1,r1,r2)->e3，重点追踪训练过程中隐式推理如何出现。

## Method / Framework

作者以 GPT-2 为主干，从 atomic triples 与 compositional queries 构造 ID/OOD、hop 数和 query-level 可控的训练环境，并用更大模型检查可扩展性。行为上区分三个阶段：early memorization、in-distribution generalization、cross-distribution generalization。机制上提出 cross-query semantic patching：把共享中间实体的 hidden state 跨 query patch，寻找可迁移的 bridge representation；再用 cosine-based representational lens 观察同一实体在不同上下文中是否聚成一致的 cosine cluster，而不把“能被 probe 解码”当作推理证据。

## Baselines / Comparisons

对照包括完整 ID/OOD triple 配置、去掉部分 ID triples、不同 ID/OOD 比例、只保留 OOD triples，以及 2-hop/3-hop query-level exposure。关键比较是 explicit decodability 与实际 generalization 的差别：如果一个实体可由 hidden state 解码，却不能支持未见组合，说明 probe 读到的是表面信息而非功能性中间变量。

## Experiments / Findings

- 行为轨迹稳定呈三阶段：先记住训练事实，再学会 ID 组合，最后才出现 OOD 表示对齐和跨分布泛化。
- ID triples 并非形成 ID generalization 的逻辑必要条件，但明显加速训练，因为 atomic query 与 two-hop query 在 r1 位置共享 causal prefix，ID supervision 直接约束了后续会使用的 hidden-state 子空间。
- 成功推理与 cosine clustering 强相关；ID-derived representation 先形成 cohesion，OOD-derived representation 后来对齐到同一 centroid。仅仅能 decode bridge entity 与 reasoning emergence 不相关，论文把“decodable ≠ functional reasoning”作为核心警告。
- 第一 hop 的 OOD generalization 其实部分是共享表示 scaffold 带来的伪泛化；第二 hop 若没有 exact compositional structure 的 query-level exposure 通常失败。3-hop 也表现为后续 hop 需要显式监督。

## Ablation / Error Analysis

ID/OOD 比例 0.8/0.2 能进入跨分布阶段，而 0.5/0.5、0.3/0.7 不能；只用 OOD triples 且没有 ID anchoring 也无法形成可用 clustering。移除部分 ID triple 后，模型仍能恢复某些原子事实，说明剩余共享实体监督在约束表示；但这不是任意组合都能迁移。错误来自 query-level structure 缺失、OOD 表示没有被拉进 ID cluster，以及把 first-hop 的 context overlap 误当作真正的算法泛化。

## Limitations

实验环境是合成 symbolic relation，不能直接代表自然语言中的知识、长上下文和工具调用。cosine cluster 是内部机制的强相关证据，但不等同于完整电路复原；大模型扩展性验证也不能消除数据分布与 query exposure 的依赖。

## 两句话总结

论文用可控 symbolic training 追踪出“记忆-同分布泛化-跨分布泛化”的隐式多跳推理发展流程，并发现功能性推理依赖跨 query 的 cosine 表示聚类。它提醒我们可解码性不是推理本身，第一跳的 OOD 成功可能是表示锚定造成的伪泛化，后续 hop 仍需精确组合监督。

## Evidence note

本笔记逐段核对 NeurIPS/arXiv PDF 的 Introduction、behavioral phases、semantic patching、cosine metrics、ID/OOD ablations 与 Discussion。

