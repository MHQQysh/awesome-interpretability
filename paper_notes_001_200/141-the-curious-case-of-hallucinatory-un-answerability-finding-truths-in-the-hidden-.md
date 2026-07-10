# 141. The Curious Case of Hallucinatory (Un)answerability

- **Authors:** Aviv Slobodkin, Omer Goldman, Avi Caciularu, Ido Dagan, Shauli Ravfogel
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.220
- **Tags:** Hidden States, Probing, Hallucination, Abstention

## Introduction

LLM 在不可回答问题上经常因过度自信而编造答案。已有研究多用 confidence 或额外 fine-tuning 判断 answerability，论文追问一个更偏 mechanistic 的问题：模型在生成幻觉答案时，内部是否已经表示“这个问题其实不可回答”？如果表示已经存在，能否从 hidden state 解码出来，并用于改进 abstention？

## Method / Framework

论文在 SQuAD 2.0、Natural Questions 和 MuSiQue 上构造 answerable / unanswerable QA。它做三组实验：prompt manipulation，在提示中明确允许回答 “not answerable”；beam scrutiny，检查 beam 中是否有拒答候选；probing，在第一个生成 token 的 hidden state 上训练 logistic regression。probe 只用每个任务 400 条 answerable 和 400 条 unanswerable 训练。

作者还用 linear concept erasure 的 projection matrix 删除 answerability subspace，再观察答案识别和 QA 性能是否变化；跨数据集测试则检验这个子空间是不是只记住某个 benchmark 的表面格式。

## Baselines / Comparisons

比较 regular prompt 与带 hint 的 prompt，zero-shot 与 few-shot，以及从输入 embedding 和最后 hidden layer 取 probe 特征。模型包括 Flan-T5-XXL 11B、Flan-UL2 20B、OPT-IML 30B。指标是 unanswerability classification F1，以及对 answerable question 的 EM / token F1，防止只提高拒答率却损害正常 QA。

## Experiments / Findings

- 在 zero-shot 下，加 hint 带来最大约 80 个 F1 点的提升。以 Flan-T5-XXL 为例，SQuAD / NQ / MuSiQue 的 classification F1 从 37.3 / 5.7 / 8.2 提升到 91.5 / 85.6 / 74.7。
- 同一个 hint 也改善 answerable / unanswerable 联合 QA；Flan-T5-XXL 的 SQuAD F1 从 58.6 提升到 89.6，NQ 从 27.0 提升到 77.0。
- 即使 regular prompt 不允许拒答，第一生成 token 的最后 hidden state 也能被线性 probe 解码；标准 prompt 下 probe 在所有模型和数据集的 F1 都超过 75%。
- 跨数据集 probe 仍有明显效果，支持 answerability 是较通用的 latent subspace，而不是单一数据集的格式记忆。PCA 也显示正确识别的 answerable / unanswerable 与 hallucinated answer 样本在表示空间中分开。
- erasure 实验表明该子空间不只是“可被读出”的信息，还会影响行为：删除后模型更难识别不可回答性，但正常 QA 也会付出代价。

## Ablation / Error Analysis

prompt hint、few-shot 示例、beam size 和 hidden layer 是主要消融。beam relaxation 允许 beam 中任何候选出现拒答，比只看 top-1 更能发现模型已经生成的 answerability signal。错误样本通常是复杂多跳 MuSiQue、问题表述不自然、上下文缺失但表面上像可回答的问题；hint 提高拒答能力，却可能降低某些 answerable 样本的 EM/F1，平均 answerable QA 有约 8.3% F1 和 7.1% EM 的下降。

## Limitations

probe 证明的是线性可读性，不等于模型真正使用该概念完成决策；erasure 的投影也可能删除与 answerability 相关的其它信息。实验只覆盖三个 QA benchmark、三种 instruction-tuned 模型，不能直接推广到闭源 API 或长对话。作者没有系统比较不同 answerability intervention 的成本，也没有解决模型知道“不可回答”却仍因解码策略生成答案的问题。

## 两句话总结

论文发现，过度自信的 LLM 往往已经在第一个生成 token 的 hidden state 中编码了问题是否可回答，只是默认 prompt 没有把这个信号转成拒答行为。提示干预、beam 检查和线性 probing 都能读出该信号，但提高 abstention 仍可能损害可回答问题的正常 QA。
