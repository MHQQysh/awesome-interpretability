# 472. Can Transformers Learn to Solve Problems Recursively?

- **Authors:** Shizhuo Dylan Zhang, Curt Tigges, Stella Biderman, Maxim Raginsky, Talia Ringer
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2305.14699](https://arxiv.org/abs/2305.14699)
- **Tags:** `mechanistic-interpretability` `program-semantics` `recursion` `shortcut-learning`

## Introduction
神经程序合成/验证模型是否真的理解递归语义，还是只学到输入模式 shortcut，是程序解释中的核心问题。论文训练小 Transformer 模拟 binary successor 和 tree traversal 等 structural recursion，并反向重建其算法与失败条件。

## Method / Framework
任务从 input-output examples 定义递归函数，再用 abstract-state machine (ASM) 表达可能的程序状态转移。作者结合 probing、position embedding swap、activation/path ablation 和对 learned computation 的规则重建，分析 encoder/decoder Transformer 是否显式表示递归深度、位置和子结构。

## Baselines / Comparisons
比较 atomic subtask 与 end-to-end recursive task、不同 positional encoding/模型方向以及自然顺序和非自然顺序数据。关注的 baseline 不是外部大模型，而是同架构在更简单任务上表现很好却在组合递归上失败的对照。

## Experiments / Findings
模型能在单个递归子任务上表现良好，但 end-to-end tree traversal 经常失败。重建出的 shortcut algorithm 可以预测某一函数 91% 的 failure cases，说明失败不是随机噪声，而是模型稳定地实现了一个不完整的近似程序。位置/递归深度信息没有以人类期待的显式变量存在，模型更多依赖 token pattern、固定深度和复制路径。

## Ablation / Error Analysis
交换 positional embeddings、隔离递归子案例和比较 atomic/end-to-end 模型显示：局部能力不保证组合能力。错误集中在递归深度变化、两个子递归分支的组合和超出训练分布的结构；这说明模型学到的是有限上下文算法而非可任意递归调用的 interpreter。

## Limitations
模型规模小、任务合成，结论不能直接外推到代码 LLM。ASM 重建包含研究者选择，91% 预测率是一个函数上的结果，不能宣称解释了所有递归行为。

## 两句话总结
论文用 mechanistic analysis 证明 Transformer 可以拟合递归样例，却常用固定深度和模式复制的 shortcut 替代真正递归。把这些 shortcut 还原成程序后能预测 91% 的失败，展示了“解释失败算法”比只报 accuracy 更有价值。

## Evidence note
已读取递归任务定义、ASM 分析、位置交换/子任务实验、失败算法重建和 limitations。
