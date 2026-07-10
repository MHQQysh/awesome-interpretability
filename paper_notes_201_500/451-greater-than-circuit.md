# 451. How does GPT-2 compute greater-than?

- **Authors:** Michael Hanna, Ollie Liu, Alexandre Variengien
- **Venue / year:** NeurIPS 2023
- **Paper:** [arXiv:2305.00586](https://arxiv.org/abs/2305.00586)
- **Tags:** `mechanistic-interpretability` `circuits` `mathematical-ability` `causal-ablation`

## Introduction
论文把“预训练模型会做简单数学”从行为描述推进到机制问题：GPT-2-small 如何判断一个年份是否大于给定的两位数。作者选用 `The war lasted from the year 1732 to the year 17` 这类自然文本，要求模型把概率质量推向大于 32 的两位年份。问题刻意不等于完整算术，因此可以在较小模型上做细粒度电路分析。

## Method / Framework
作者先用 120 个名词、约 10K 个 year-span 样本确认行为，再以 path patching、activation ablation、logit attribution 和 attention-pattern analysis 搜索最小子图。电路包含读取 `YY` 的中层注意力头，以及最后几层 MLP；MLP 9--11 尤其直接把“更大的两位数”写入 logits。标准模型的 probability difference 为 81.7%，cutoff sharpness 为 6.0%。

电路语义是：注意力头从 `YY` 位置取出阈值，随后 MLP 在不同上下文中把这一阈值转换为对候选年份的增益/抑制。它不是一个单独的“greater-than neuron”，而是跨层、跨头、跨 MLP 的路径组合。

## Baselines / Comparisons
作者比较完整 GPT-2、只保留候选电路的 patched model、反向 patch 的破坏模型，以及与原电路规模和位置相近但路径不重叠的随机/替代电路。比较重点不是与外部数学模型比准确率，而是验证电路的充分性、必要性和路径依赖。

## Experiments / Findings
保留电路路径、其余输入使用干扰数据时，probability difference 仍为 72.7%，约为完整模型的 89.5%，cutoff sharpness 为 8%。把电路节点换成干扰数据、模型其余部分保留正常输入时，差值变成 -36.6%，说明电路既大体充分又具有必要性。电路在不同句法/名词上下文中被激活，说明机制介于死记硬背与真正可迁移数学规则之间。

## Ablation / Error Analysis
去掉注意力头与 MLP 之间的关键路径后性能明显下降；只保留一个重叠路径会恢复部分行为，MLP 重叠通常比注意力头重叠影响更大。作者还发现模型会激活与年份、数量和比较关系相关的不同上下文特征，因此不能把结果简化为一个固定词表匹配。

## Limitations
任务只覆盖有限的年份模板和 GPT-2-small，不能推出模型拥有普遍数学能力。电路仍是人工筛选后的近似子图，patching 结果受候选节点和度量影响；“中间状态代表语义”也不等于完整算法证明。

## 两句话总结
这篇论文用因果电路分析解释了 GPT-2 如何在自然文本中实现一个有限的 greater-than 运算。关键机制是注意力头读取阈值、后层 MLP 将阈值转化为候选年份 logits，且该电路在干扰 patch 下表现出近似充分性和必要性。

## Evidence note
已逐段读取本地 PDF 转写，重点核对了任务构造、path-patching 电路评估、组件语义和数值结果。
