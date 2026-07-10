# 404. Towards Automated Circuit Discovery for Mechanistic Interpretability

- **Authors:** Arthur Conmy, Augustine N. Mavor-Parker, Aengus Lynch, Stefan Heimersheim, Adrià Garriga-Alonso
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2304.14997
- **Tags:** `mechanistic-interpretability` `circuit-discovery` `ACDC` `causal-ablation`

## Introduction
机制解释常靠研究者手工提出组件、做 activation patching、不断试错，难以扩展到更多层和更大模型。本文提出 ACDC/automated circuit discovery 的方向：从一个行为任务和 clean/corrupted input 对出发，自动保留能恢复目标行为的 attention/MLP edges，生成候选 circuit。

## Method / Framework
算法从完整计算图开始，按照边的重要性和行为恢复效果迭代删除。对一个候选 edge 做 ablation/patching，若删除后 corrupted output 仍接近 clean target，就移除该 edge；否则保留。输出是稀疏的有向 subgraph，研究者再对其做 faithfulness 和 completeness 检查。

## Baselines / Comparisons
比较包括手工电路分析、随机/贪心 edge selection、完整模型和不同 threshold/metric 的自动发现。任务主要是 induction、IOI 等可控 Transformer behaviors；指标包括 circuit faithfulness、边数量/稀疏度、运行时间和对目标输出的恢复。

## Experiments / Findings
ACDC 能在小型语言模型行为上自动找出与人工已知 circuit 相似的结构，并在保持行为的同时显著稀疏化计算图。它降低了 circuit discovery 的人工门槛，为从“少数案例的手工解释”走向自动化 pipeline 提供证据。

## Ablation / Error Analysis
阈值、edge scoring、corruption distribution 和 circuit size 是主要消融。过 aggressive 的删除会让 circuit faithfulness 下降；过保守则保留大量无关边，自动化收益变小。只在单一 corruption 或单一 prompt 上通过不代表 circuit 对分布外输入和真实任务忠实。

## Limitations
ACDC 的搜索成本随模型和边数增长，且依赖选择好的 clean/corrupted pairs、metric 和行为假设。发现的 circuit 可能是 sufficiency circuit 而非唯一真实机制；算法更适合小模型和明确行为，尚不能直接解释开放式大模型知识/推理。

## 两句话总结
ACDC 用逐边 ablation/patching 自动删除对目标行为非必要的连接，生成稀疏、可检查的 Transformer circuit。它把机制发现从手工试错推进到算法化，但结果仍依赖任务、corruption、阈值和 faithfulness 设计，不能视为唯一因果分解。

## Evidence note
已读取 arXiv 2304.14997 v4 本地 PDF 的方法、自动删除策略、faithfulness/sparsity 对比和局限章节。
