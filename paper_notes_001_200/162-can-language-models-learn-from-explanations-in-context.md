# 162. Can Language Models Learn from Explanations in Context?

- **Authors:** Andrew K. Lampinen et al.
- **Venue:** EMNLP Findings 2022
- **Paper:** https://aclanthology.org/2022.findings-emnlp.38
- **Tags:** In-context Learning, Explanation, Few-shot, Scaling

## Introduction

few-shot prompting 通常给 input-output pairs，但人类学习常依赖解释来连接例子和任务原理。论文研究 explanation 是否真的帮助 LM 学会新任务，而不是把 explanation 当成给用户看的 post-hoc artifact。

## Method / Framework

作者为 40 个 challenging tasks 标注 answer explanations，并制作 matched control explanations。测试 zero-shot / few-shot、instruction、examples 与 explanations 的不同组合，使用 hierarchical multilevel model 处理 task、prompt、model 的嵌套依赖。还在小验证集上 hand-tune explanation，比较只选 examples 与 jointly selecting examples + explanations。

## Baselines / Comparisons

比较无 explanation、human explanation、matched control、task instruction、随机 / hand-tuned explanation、只选 examples 和同时选 example-explanation pair。模型覆盖不同规模 LM，重点检验 scaling effect。

## Experiments / Findings

- explanation 能在不 fine-tuning 的情况下提高 few-shot performance；even untuned explanation 超过 matched controls。
- 在小验证集上 hand-tuned explanation 带来更大收益；同时选择 examples 和 explanations 明显优于只选 examples。
- 解释的帮助来自 example 与 explanation 的连接，而不是长度、词汇或格式等低级特征。
- 只有大模型稳定受益，小模型可能无法解析长 reasoning 或把 explanation 当作普通文本。

## Ablation / Error Analysis

主要消融 explanation 类型、control、instruction、task description、example selection 和模型规模。解释过长、与答案不一致或只复述表面线索会伤害效果；不同任务收益差异较大，说明 explanation 不是一个脱离任务的通用 prompt token。

## Limitations

40 个任务和人工 explanation 仍有主观性，结果不能证明模型真正理解了人类解释。大规模模型成本高，prompt 选择本身可能过拟合 validation set；未覆盖长文档、多轮交互和语言间迁移。

## 两句话总结

论文显示，解释不仅能帮助人读模型，也能作为 in-context learning 的任务原理信号，提高大模型 few-shot 性能。收益依赖模型规模、解释质量和 example-explanation 配对，不能简单归因于增加上下文长度。
