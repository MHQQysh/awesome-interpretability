# 461. Does Localization Inform Editing?

- **Authors:** Peter Hase, Mohit Bansal, Been Kim, Asma Ghandeharioun
- **Venue / year:** NeurIPS 2023
- **Paper:** [arXiv:2301.04213](https://arxiv.org/abs/2301.04213)
- **Tags:** `causal-tracing` `knowledge-editing` `localization` `ROME` `MEMIT`

## Introduction
ROME/MEMIT 先用 Causal Tracing 找事实所在的 MLP 层，再在那里编辑，背后的直觉是“改变知识应当改存储知识的位置”。论文直接检验这个前提：representation denoising 的 localization effect 是否能预测某一层的 factual edit success。

## Method / Framework
在 GPT-J + CounterFact 上逐层比较 Causal Tracing 的 fractional restoration effect 与 ROME Rewrite Score，并补充 MEMIT、Adam fine-tuning、Paraphrase/Neighborhood score、GPT2-XL、ZSRE 和 representation zeroing。编辑问题除 error injection 外，还包括 tracing reversal、fact erasure、fact amplification 和 fact forcing。

## Baselines / Comparisons
主比较是 layer choice、tracing effect、二者交互对 edit success 的回归解释力；编辑器包含 ROME、MEMIT 与 Adam-based editing。实验覆盖默认 ROME layer 6 和多个层位，而不是只报告一个调参后的成功率。

## Experiments / Findings
GPT-J CounterFact 的 652 个样本上，layer-6 tracing effect 与 Rewrite Score 的相关系数为 rho=-0.13（p<1e-3），并非正相关。回归中 layer choice 的 R2 为 0.947，tracing effect 仅 0.016；加入 tracing 后 combined R2 只有 0.948，说明在控制层位后 localization 只解释约 0.1% 额外方差。作者还发现相当部分事实并不位于 ROME/MEMIT 常编辑的层范围。

## Ablation / Error Analysis
结论对 ZSRE、representation zeroing、不同 tracing window、GPT2-XL 和其他 edit metrics 稳健。一个关键 nuance 是，某些更接近 tracing 输入/目标的任务（如 fact forcing、amplification）与 tracing 的关系会变强，但“注入新事实”与“事实原来存在哪里”仍不能等同。层位本身是强预测变量，可能反映读出/传播的可编辑性，而非知识存储位置。

## Limitations
实验模型和 CounterFact/ZSRE 任务有限，相关性不能证明所有编辑机制。Causal Tracing 的窗口、噪声方式和 Rewrite Score 也会影响数值；论文推翻的是一种普遍选层假设，不是说 localization 对任何行为都无用。

## 两句话总结
论文用逐层因果与编辑对照证明：知道哪一层对事实表示最敏感，并不能告诉你在哪一层编辑最有效。对 GPT-J 而言层位选择远比 tracing effect 重要，这提醒 mechanistic localization 与可控行为修改是两个不同问题。

## Evidence note
已读取主实验、R2 表、四类 editing variant、ZSRE/zeroing robustness 与作者对 layer-mediated editing 的讨论。
