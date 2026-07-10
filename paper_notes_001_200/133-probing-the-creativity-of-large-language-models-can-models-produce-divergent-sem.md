# 133. Probing the “Creativity” of Large Language Models: Can Models Produce Divergent Semantic Association?

- **Authors:** Honghua Chen, Nai Ding
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.858
- **Tags:** Probing, Creativity, Behavioral Evaluation, Decoding

## Introduction

LLM 的 next-token prediction 看起来像是在复现训练分布，因此它能否真正产生“新颖而有价值”的内容仍不清楚。直接让人评价艺术或科学创作会混入主观性、领域知识和数据泄漏，Alternate Uses Task 还依赖物体选择。作者于是把问题收窄到 creativity 的一个可客观测量的组成部分：divergent thinking，即能否生成彼此不相关、远距离的语义概念。

论文的可检验问题是：不同规模和训练方式的 LLM 在一个与人类研究一致的语义发散任务上表现如何？greedy、top-p 和 temperature 是否改变这种能力？高分究竟来自远距离联想，还是只是在生成罕见词？

## Method / Framework

作者采用 Divergent Association Task（DAT）：让模型生成 10 个互不相关的名词，过滤非名词后取前 7 个有效词，用 GloVe 向量计算所有词对的平均 cosine distance，再乘 100 得分。分数高代表词之间的语义关联更远，但它只测“little-C”日常创造力中的发散联想，不测价值、情绪、动机或完整作品。

模型包括 GPT-4、GPT-3.5-Turbo、Oasst-Llama-30B、Vicuna-13B、ChatGLM-6B。确定性设置使用 greedy；随机设置使用 top-p=0.9、temperature=0.7，并单独扫描 temperature 0.1 到 1。作者还计算 surprisal（负 log 词频）以检查 DAT 是否只是词频效应。

## Baselines / Comparisons

比较对象包括 8,572 名人类参与者的 DAT、从 WordNet 随机抽取名词的 Random baseline、只要求“写 10 个名词”的 Base prompt，以及 GPT-4 / GPT-3.5 等不同规模和训练方式的 LLM。还在相同 surprisal 下比较模型和人类，避免把“生成冷门词”误当作创造力。

## Experiments / Findings

- Greedy 下 GPT-4 的 DAT 为 89.1，超过 96.1% 的人类；GPT-3.5-Turbo 为 80.8，高于平均人类水平。其它模型整体更低，并大致随模型规模上升。
- 对 GPT-4 以外的模型，top-p sampling 通常比 greedy 得到更高 DAT，但方差也更大，偶尔生成低分或无效答案；随机性带来发散，也牺牲稳定性。
- temperature 从 0.1 增大到 1 对多数模型有正效应，但收益有限，过高温度会造成不稳定和非法输出。GPT-4 的 greedy 已经较好，额外随机化反而不一定有益。
- WordNet 随机 baseline 可以超过多数模型和人类，因为它完全不受自然语言分布约束；这正说明 DAT 高分本身不是完整语言创造力。
- 在控制 surprisal 后，GPT-4 和 GPT-3.5 仍超过平均人类，但 GPT-4 的优势减弱；其它模型低于平均人类，说明稀有词频是一个真实混杂因素。
- Base prompt 产生的词更相关；DAT 和 Random prompt 能诱导模型主动调节分布，生成更远的联想。

## Ablation / Error Analysis

主要消融是 decoding 和 prompt 条件：greedy、top-p、不同 temperature、Base、Random、DAT。作者还检查了每个模型的样本量收敛，以及用 GloVe 之外的语义距离设置，结论基本一致。最关键的反例是 WordNet Random：它的高 DAT 证明“语义距离”会奖励没有语言任务约束的随机词，因此必须结合 surprisal、人类分布和有效名词率解读。

## Limitations

创造力定义本身有争议；DAT 只覆盖发散联想，不代表新颖性与价值同时成立，也不能直接泛化到写作、科学发现或艺术创作。GloVe 的词向量空间、名词过滤和训练数据泄漏都会影响分数；模型输出中的 invalid words 被过滤也可能带来选择偏差。作者没有研究多轮创作、领域知识、长期规划或人机共创。

## 两句话总结

论文用 DAT 把 LLM 的“创造力”转化为可重复的语义发散测试，发现 GPT-4 的 greedy 联想已经达到很高的人类相对水平。随机解码能提高多数模型的发散分数，却引入不稳定和词频混杂，因此高 DAT 只能说明远距离语义关联，不能单独证明模型具备完整创造力。
