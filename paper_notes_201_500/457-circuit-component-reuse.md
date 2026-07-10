# 457. Circuit Component Reuse Across Tasks in Transformer Language Models

- **Authors:** Jack Merullo, Carsten Eickhoff, Ellie Pavlick
- **Venue / year:** ICLR 2024
- **Paper:** [arXiv:2310.08744](https://arxiv.org/abs/2310.08744)
- **Tags:** `mechanistic-interpretability` `circuit-reuse` `path-patching` `IOI` `task-generalization`

## Introduction
电路分析常被批评为 task-specific：每个任务都找到一套 head，无法说明模型更一般的算法。论文测试一个更强的假设：看似不同的 IOI 与 Colored Objects，是否复用“从候选集合中选出并复制一个 token”的通用子电路。

## Method / Framework
在 GPT2-Medium 上用 path patching、attention pattern analysis 和 logit attribution 追踪直接影响 logits 的 heads，再反向查找影响这些 heads 的路径。IOI 中 Duplicate/Induction heads 识别重复 subject，Inhibition heads 抑制 S1/S2，Mover heads 复制剩余 IO；Colored Objects 中 Duplicate heads 识别对象，Content Gatherer heads 把问题对象信息写入末端，Mover heads 从三种颜色中复制正确颜色。

## Baselines / Comparisons
首先复现 Wang et al. 在 GPT2-Small 上的 IOI circuit 到 GPT2-Medium；然后对 Colored Objects 与 IOI 做同阈值电路重叠比较。还用随机选择的非 content-gatherer heads 作干预对照，并以模型原始准确率和手工“理想修复”准确率对照。

## Experiments / Findings
GPT2-Medium 在 IOI 上复现了 Duplicate/Induction、Inhibition、Mover 和 Negative Mover 的结构。Colored Objects 原始准确率只有 49.6%，但其前 2% 重要 heads 与 IOI 有 25/32、约 78% 重叠；差异主要是用三枚 content-gatherer 替代 inhibition 与 negative mover。阻断 content gatherer 传给末端的问题信息后准确率降到 35%，随机 head 对照约 49.9%±1.0。

作者把四个中层 heads 调整为更像 IOI 电路的状态后，Colored Objects 准确率从 49.6% 提升到 93.7%。下游 mover heads 的变化方向符合 IOI 电路预测，说明不仅是 head 名称重合，而是子电路交互规则在不同输入任务中复用。

## Ablation / Error Analysis
Negative Mover 在 GPT2-Medium 中主要 demote S2，而非对所有名字均匀抑制，显示同一类 head 的具体实现随模型/任务改变。阻断问题中的对象/颜色信息会让 mover 在三种颜色之间近似随机选择，验证 content gatherer 的因果作用；替代随机 heads 不造成同等损失。

## Limitations
电路重叠的阈值和“哪些 heads 属于电路”没有唯一答案，作者也承认量化 overlap 困难。结果只覆盖 GPT2-Medium 的两个人工构造任务，不能直接推及大模型或所有 task-general circuits。

## 两句话总结
论文用因果 path patching 证明 IOI 与 Colored Objects 复用了约 78% 的关键 attention heads，并共享“抑制/聚合信息后复制候选 token”的算法骨架。把 Colored Objects 的部分 heads 修复成 IOI 风格后准确率跃升到 93.7%，为 task-general circuit reuse 提供了比静态相关性更强的证据。

## Evidence note
已逐段核对两任务定义、path-patching 方法、head 角色、78% overlap、35%/49.9% 对照和 49.6%→93.7% 干预结果。
