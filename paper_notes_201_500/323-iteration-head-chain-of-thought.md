# 323. Iteration Head: A Mechanistic Study of Chain-of-Thought

- **Authors:** Charles Arnal, Wassim Bouaziz, Vivien Cabannes, François Charton, Julia Kempe
- **Venue / year:** 2024 preprint
- **Paper:** https://doi.org/10.52202/079017-3463
- **Tags:** Chain-of-Thought, Iteration Head, Mechanistic Interpretability, Reasoning

## Introduction

CoT 生成不是一次性映射：模型需要读取前一步输出，把它作为下一步输入，反复执行相似的 reasoning operation。论文研究这种循环是否由可定位的 iteration head/circuit 实现，而非把 CoT 当作神秘的整体能力。

## Method / Framework

作者在可控的多步 reasoning transformer 上追踪中间步骤、attention patterns、residual representations 和 output logits，寻找将当前状态与前一步状态连接起来的 iteration head。通过 activation/path patching、head ablation 和 step-by-step causal tracing，分析模型如何重复相同运算并把结果写回序列。

## Baselines / Comparisons

比较 full model、无 iteration head/候选 head ablation、不同 reasoning steps/sequence length、direct answer 与 CoT prompting。评价是每一步和最终答案的 accuracy、logit contribution、circuit faithfulness，而非仅比较是否生成更长文本。

## Experiments / Findings

研究将 CoT 的循环性归因于一类 iteration head：它读取先前步骤的状态/答案，把中间结果复制或变换到当前计算位置，使同一算法可以跨多步复用。随着 steps 增加，若该 head/circuit 被破坏，模型会在中间步骤后失去状态传递；这为 CoT 的“迭代执行”提供了机制视角。

## Ablation / Error Analysis

单个 head 可能有 redundancy，需组合 ablation 和 path patching；模型生成的文字步骤可能与真实 hidden-state iteration 不一致。step position、tokenization、reasoning algorithm 和 context length 影响 iteration path，不能把一个 toy circuit 直接当作所有 CoT 的通用机制。

## Limitations

全文/出版版本实验细节需进一步核对，且控制任务与自然语言 CoT 有明显距离；iteration head 解释了状态复用，不等于解释每个数学/常识算法的内容。

## 两句话总结

论文把 CoT 视为可循环执行的状态传递过程，并用 iteration head/circuit 解释模型如何把前一步结果带入下一步。它将“长答案”与“真实迭代计算”分开，但机制能否跨任务和大模型泛化仍需更多因果验证。

## Evidence note

基于题录和可获得的预印本信息整理；具体章节/数值待正式 PDF 完整读取后再补强。

