# 421. Tracr: Compiled Transformers as a Laboratory for Interpretability

- **Authors:** David Lindner, János Krámár, Sebastian Farquhar, Matthew Rahtz, Vladimir Mikulik
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2301.05062
- **Tags:** `mechanistic-interpretability` `compiler` `RASP` `ground-truth`

## Introduction
真实 Transformer 的 learned program 未知，解释方法很难知道自己的答案是否正确。Tracr 提出把人类可读的 RASP 程序编译成标准 decoder-only Transformer，得到内部结构和 ground-truth computation 已知的模型，用来开发、教学和评估 interpretability tools。

## Method / Framework
编译器把 RASP expression graph 转成 embedding、attention、MLP、residual stream 和 output weights：先建计算图，再分配变量/类型的 residual subspace，最后生成 Transformer 权重。论文展示 fraction-of-previous、sort unique、balanced parentheses 等程序，并可通过 gradient descent 压缩 compiled model 研究 superposition。

## Baselines / Comparisons
比较原始 RASP/Tracr model、压缩后的 learned Transformer、人工已知程序和解释工具输出。Tracr 不是性能 baseline，而是 ground-truth testbed：若解释器恢复的 circuit 与编译程序一致，才有可验证的 faithfulness。

## Experiments / Findings
Tracr 能编译一系列离散算法，compiled model 的 residual stream 和组件功能可直接读出。压缩 frac_prevs/sort_unique 模型后，低维 residual stream 仍能保持任务性能并产生 superposition，为研究“已知程序如何被神经网络压缩”提供 controlled setting。

## Ablation / Error Analysis
embedding dimension、是否压缩、程序复杂度和 attention/MLP component 是主要消融。压缩后结构可能不再忠实于原 RASP；模型虽行为正确，内部实现可能变成替代电路，因此解释工具需要同时测行为和机制相似性。

## Limitations
RASP/Tracr 的表达力、效率和现实性有限，不能编译完整 LLM；正交 subspace 还会浪费维度，手工矩阵与真实训练过程不同。Tracr 结果应视为 interpretability 的 minimum bar，不应直接外推真实大模型。

## 两句话总结
Tracr 把 RASP 人类程序编译成 Transformer 权重，为可解释性方法提供已知结构和 ground truth。它特别适合验证 circuit discovery、superposition 和 probing，但编译模型的简化与非自然训练方式限制了现实外推。

## Evidence note
已读取 arXiv 2301.05062 v5 本地 PDF 的 compiler、RASP examples、compression/superposition case study 和 limitations。
