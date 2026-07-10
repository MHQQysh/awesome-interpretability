# 035. Enhancing Semantic Consistency of Large Language Models through Model Editing: An Interpretability-Oriented Approach

- **作者 / venue**：Jie Li, et al.；Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.199/)
- **任务**：让语义等价 prompt 得到一致输出，并用模型编辑定位相关内部组件

## 1. Introduction

LLM 可能对语义等价但表面不同的 prompt 给出不一致答案。传统 prompt consistency/SFT 方法通常通过增加 paraphrase 数据训练整个模型，成本高且难以说明哪些内部组件造成不一致。论文把 semantic consistency 当成 model editing 问题：找到对一致性影响最大的 component，再以局部 activation 方向修正。

## 2. Method

- 用 GPT-4 生成语义等价 prompt pairs。
- 对 target LLM 的输出进行比较，构造 consistency objective。
- 通过 interpretability analysis 定位 top-K model components/activations，并计算编辑方向。
- 在推理时对 activation 做调节，不改变模型原始参数；编辑强度 α 控制 consistency 与能力保持之间的折中。

## 3. Baseline 与对比

- 原始 Llama2-7B-Chat。
- SFT/prompt consistency 方法：用 paraphrase prompt 训练模型。
- 随机选择 editing directions 的对照，检验收益是否来自“编辑”本身。
- 多个语义一致性与下游任务数据集，包括 RobustMRPC；同时做 out-of-domain generalization。

## 4. Findings

- 按 interpretability 定位的 top-K components 能改善 semantic consistency，并保持/提升部分任务性能。
- 随机编辑方向会损伤 consistency 和 task performance，说明模型编辑方向不是任意可交换的。
- α 太大时（文中示例约 α=9.0）性能下降；中等强度（如 α=3.0）在部分设置优于基线，表明编辑强度必须校准。
- 方法在 out-of-domain prompt 上仍有一定泛化，支持编辑的是较稳定的 semantic behavior，而非某一条 prompt 的记忆。

## 5. Ablation 与局限

需要同时报告 consistency、原任务准确率、编辑后未目标行为和跨 prompt 泛化。activation editing 可能只修正输出层表现，不能证明已经找到完整语义电路；生成 paraphrase 的 GPT-4 也可能带来数据偏差。

**一句话评价**：论文把“prompt 换个说法结果变了”转成局部模型编辑问题，展示 interpretability 可以帮助选择编辑方向，但编辑强度和 collateral damage 仍是核心风险。
