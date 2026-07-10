# 124. Don’t Just Say “I don’t know”! Self-aligning Large Language Models for Responding to Unknown Questions with Explanations

> 人工精读笔记：EMNLP 2024。重点是 unknown question detection、unknown reason explanation 和小样本迭代 self-alignment。

## 0. 论文信息

- **作者**：Yang Deng, Yong Zhao, Moxin Li 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.757)
- **模型/数据**：LLaMA2、Vicuna；QNotA、KUQP 等 known/unknown QA

## 1. Introduction：要解决什么问题

面对模型知识边界之外的问题，简单说“I don’t know”无法解释为什么不知道，也不能区分问题无答案、证据不足、时效过期或概念不理解。反过来，模型若强行回答会产生 hallucination。

## 2. Method / Framework：怎么解决

Self-Aligned 用少量 seed data 让模型先识别 unknown category，再生成带理由的拒答/不确定回答，并把自身反馈迭代用于 self-alignment。输出不仅是 known/unknown 标签，还要解释导致未知的原因。

## 3. Baseline / 对比基线

Zero-shot、Few-shot、Proactive、ProCoT、R-Tuning/普通 fine-tuning，以及 vanilla Vicuna/LLaMA2。对照 unknown detection、unknown reason classification 和 open-ended response quality。

## 4. Experiments / Findings：结果如何读

Self-Aligned 在不同 unknown categories、两个 base models 和多个数据集上总体超过 zero-shot，并在许多类别超过 fine-tuning baselines。表 2 中 Vicuna 的 F1/分类指标提升明显；自动与人工评价都显示回答更相关、理由更具体、幻觉更少。

方法只需很小 seed data，迭代后不只是重复“我不知道”，而是区分 lack of knowledge、ambiguous question、insufficient context 等原因。多轮迭代到稳定点后收益趋于饱和，说明自对齐不是无限收益。

## 5. Ablation / 机制解释

单轮 vs iterative self-alignment、无 explanation vs 有 reason、不同 seed size、不同 base model 构成主要消融。解释标签对 unknown detection 有辅助作用，说明拒答质量与原因建模需要共同训练。

## 6. Limitations / 局限与复现注意

unknown 标签和原因依赖数据构造/LLM judge；模型可能学会模板化拒答；开放域真实问题的“未知”边界动态变化；过度拒答会损害 recall。

## 7. 两句话总结

Self-Aligned 让 LLM 不仅判断问题未知，还解释未知原因，并用少量 seed data 迭代自对齐以减少强行作答 hallucination。它在 QNotA/KUQP 和 LLaMA2/Vicuna 上优于 zero/few-shot 与若干 fine-tuning 基线，但仍需平衡拒答与回答覆盖率。
