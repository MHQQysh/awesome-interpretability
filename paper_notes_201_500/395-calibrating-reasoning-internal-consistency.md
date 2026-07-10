# 395. Calibrating Reasoning in Language Models with Internal Consistency

- **Authors:** Zhihui Xie, Jizhou Guo, Tong Yu, Shuai Li
- **Venue / year:** NeurIPS, 2024
- **Paper:** https://arxiv.org/abs/2405.18711
- **Tags:** `reasoning` `internal-state` `calibration` `CoT` `probing` `interpretability`

## Introduction
CoT 往往提高 answer accuracy，却可能包含明显错误、互相矛盾或与 final answer 不一致的 rationale。只看最终文字无法判断 reasoning path 是否可靠；作者因此研究中间层对答案 token 的 latent prediction，并问：模型各层是否“同意”最终答案，这种 agreement 能否作为 confidence signal？

## Method / Framework
定义 **internal consistency (IC)**：在生成 reasoning path 后，从多个 intermediate layers 解码答案 token，统计这些 latent predictions 与最终 stated answer 一致的比例。高 IC 表示中间层和最终层方向一致，低 IC 表示内部表征冲突。

作者还提出 SC+IC：先像 self-consistency 一样采样多个 reasoning paths，再用每条 path 的 IC 对候选答案加权，不把所有路径等权投票。实验分析不同层的 attention 和 FFN，寻找 internal inconsistency 出现的位置和模式。

## Baselines / Comparisons
对比包括 direct answer、CoT、self-consistency (SC)、不同 prompting/decomposition strategies 和不使用 IC 的普通 majority vote。实验覆盖多个 decoder LLM、Mistral 系列和多个 reasoning 数据集，包括 GSM8K、MATH、AQuA、SVAMP、MultiArith、PrOntoQA 等。

评价使用 accuracy/calibrated accuracy、正确/错误 path 的 IC 分布、layer-wise latent probe accuracy 和 calibration plot，而非只看 final answer accuracy。

## Experiments / Findings
作者发现 CoT 提升最终答案准确率，但 middle-layer 与 final-layer representation 的冲突也更明显；因此“更长 rationale”不自动意味着内部更一致。IC 与 answer correctness 有强相关，能把正确与错误 reasoning paths 区分开，且不需要额外训练的 confidence head。

SC+IC 在多模型、多任务上稳定优于普通 SC；Table 1 报告的是跨 10 个随机种子的 calibrated accuracy。它通过给高 IC path 更高权重，减少了错误但表面合理的 rationale 对多数投票的影响。attention 分析还显示中间层对 query/reasoning token 的关注与正确性相关，FFN/value directions 可能推动相互冲突的答案 token。

## Ablation / Error Analysis
层聚合是关键消融：把所有 layers 等权合并并不总最优，作者进一步学习/选择 aggregation strategy。不同 prompting、模型规模和任务的 IC pattern 不完全相同；在某些任务上 CoT 产生更低 IC，说明 reasoning 的语言化过程可能引入新的不一致。

probe accuracy 只能说明 answer information 可从 activation 中读出，不能说明模型真正使用了该信息；IC 也可能与 final answer 的 token bias 相关。作者用 Table 2 的 PrOntoQA path 例子说明低 IC path 往往对应错误 rationale，但仍需干预实验才能证明 IC 是因果信号。

## Limitations
方法需要访问中间层和答案 token，不适用于纯黑箱 API；当前主要是 decoder-only 模型和相对短的 reasoning tasks。IC 可能受层数、tokenization、答案格式和 aggregation 超参数影响，不能直接等价于人类 confidence。作者也指出 encoder-decoder 和更复杂任务留待未来。

## 两句话总结
Internal consistency 用中间层对答案的 latent predictions 与最终答案的一致比例，读出 CoT reasoning path 的内部冲突，并将其用于加权 self-consistency。它提供了比自然语言 rationale 更接近模型内部的 calibration signal，但 probe 可读性、层聚合和因果忠实性仍需进一步验证。

## Evidence note
已读取 arXiv 2405.18711 v2 本地 PDF 的摘要、IC 定义、SC+IC、Table 1、attention/FFN 分析和 limitations。
