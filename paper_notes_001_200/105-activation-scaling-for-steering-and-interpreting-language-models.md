# 105. Activation Scaling for Steering and Interpreting Language Models

> 人工精读笔记：Findings of EMNLP 2024。重点是用每个模型位置一个 scalar 替代高维 steering vector，并把 steering 位置变成可解释地图。

## 0. 论文信息

- **作者**：Niklas Stoehr, Kevin Du, Vésteinn Snæbjarnarson 等
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.479)
- **方法**：ACTIVSCALAR、dynamic activation scalars
- **任务**：concept steering、counterfactual/knowledge conflict 等小型 mechanistic datasets

## 1. Introduction：要解决什么问题

常见 steering vector 在很多 layer/token/head 上加入高维向量，能改变模型行为却难以知道哪个组件真正负责；参数多，也容易把 steering direction 与 activation magnitude 混在一起。作者提出只学习一个 scalar，乘在已有 activation vector 上：方向不变，只改变该位置计算的强弱。

这样既可以用少量参数控制模型朝向目标概念，又能把 learned scalar 直接画成 layer/token/head 的 intervention map，回答“模型在哪里已经有相关方向、哪些位置需要增强或抑制”。

## 2. Method / Framework：怎么解决

给每个候选 activation location 一个可学习系数 beta，干预形式是 beta × activation；beta=0 表示不干预，正/负或大/小值表示增强/抑制原有计算。训练目标直接优化 steering effectiveness，例如把 Italy steering 到 France 或处理 conflict dataset。

ACTIVSCALAR 将 intervention tied 到 prompt position、layer、attention head/MLP 等局部位置。Dynamic Activation Scalars 进一步让 scalar 依赖当前 activation，形成输入条件化的 gate，而不是所有 prompt 使用固定系数。

## 3. Baseline / 对比基线

- **STEERVEC**：学习并添加高维 steering vector，是主要 baseline。
- **ACTIVPATCH/ATTRPATCH**：activation patching 或 attribution-based intervention。
- **固定 scalar vs dynamic scalar**：检验是否需要输入条件化。
- **不同 layer/head/MLP location**：定位 steering 的结构。
- **合成 conflict datasets/模板泛化**：检验 steering 能否从训练模板迁移到测试模板。

## 4. Experiments / Findings：结果如何读

Activation scaling 在 steering effectiveness 上整体可与 additive steering vector 相比，同时需要的可学习参数少得多。它在 conflict/counterfactual 数据上可以增强目标答案或抑制冲突答案，并且 scalar map 直接显示某些中层 attention/MLP 是关键 gate。

重复训练时，scalar location pattern 比高维 vector direction 更容易解释：多个随机种子往往在相似 layer/position 找到 intervention，而不是完全随机分布。Dynamic Activation Scalars 在混合任务上能根据输入改变干预强度，减少固定 scalar 对单一模板过拟合。

作者也比较 activation scaling 与 activation patching/gradient attribution：gradient-based learning 通常更快，且能同时取得 steering 与 attribution；但参数强度、L1 正则和位置选择对最终效果非常敏感。

## 5. Ablation / 机制解释

- 添加向量 vs 缩放已有 activation，分离 direction change 与 magnitude gating。
- fixed vs dynamic scalar，验证输入条件化价值。
- 各 layer/head/MLP 单独训练，观察最有效位置和跨任务重叠。
- L1 regularization sweep，控制 intervention 稀疏性和稳定性。
- train template/test template，对照 steering 泛化。

## 6. Limitations / 局限与复现注意

- 主要实验规模较小、任务较合成，不能直接推出大模型真实对话 steering。
- 只改变 activation magnitude 可能无法表达需要新方向的行为。
- scalar map 可读不等于它是唯一 causal circuit；多个位置可能冗余。
- 参数正则、候选位置和 post-hoc beta 选择会显著影响结果。

## 7. 两句话总结

ACTIVSCALAR 用每个模型位置一个可学习 scalar 去增强或抑制已有 activation，既以远少于 steering vector 的参数实现行为控制，也给出可视化的机制位置。实验显示其 steering 效果与向量方法相当、dynamic 版本更灵活，但证据主要来自小型合成任务和特定位置搜索。
