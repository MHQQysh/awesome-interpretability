# 253. Synergizing Large Language Models and Knowledge-Based Reasoning for Interpretable Feature Engineering

- **Authors:** Mohamed Bouadi, Arta Alavi, Salima Benbernou, Mourad Ouziri
- **Venue / year:** WWW 2025 poster
- **Paper:** https://openreview.net/forum?id=lZ3jDGOR2Y
- **Tags:** ReaGen, Feature Engineering, Knowledge Graph, Neuro-Symbolic, Tabular ML

## Introduction

表格任务中的 feature engineering 通常需要领域专家逐个设计变换，耗时且难以迁移；单纯让 LLM 生成特征又容易幻觉、重复或使用不可解释的长上下文。论文要解决的是如何在自动化 feature engineering 中同时获得预测收益和面向领域专家的可解释语义。

## Method / Framework

ReaGen 是 KG 与 LLM 的协同 pipeline。它首先根据数据集描述在知识图谱上做 symbolic reasoning，检索与字段/领域相关的概念和关系；然后让多个 LLM 以迭代方式提出有名字、有语义的候选特征；最后再次用 KG 进行逻辑验证，过滤不满足关系约束、无法由知识支持或语义不一致的候选。输出不仅有特征值，还带有生成理由和特征 utility，便于人检查。

## Baselines / Comparisons

作者将 ReaGen 放在 AutoML/自动特征生成流程中，与传统人工/规则 feature engineering、常规自动特征方法以及仅用 LLM 生成的方案比较，核心评价是下游预测性能和生成特征的可解释性。由于 OpenReview 公开版本的表格细节与最终出版版存在版本差异，本笔记不把摘要中的“significant improvement”转换成未经核对的具体百分点。

## Experiments / Findings

论文在多个公共表格数据集上验证，观察到 KG 检索给 LLM 提供了领域上下文，LLM 负责把关系转成自然语言可理解的候选，KG 逻辑检查则降低了幻觉特征进入训练集的概率。作者报告 ReaGen 在预测准确率和解释性上优于对照，并能为每个特征输出 feature utility explanation。它的实质贡献不是让 LLM 独立“猜”一个变换，而是把候选生成与符号约束放在闭环中。

## Ablation / Error Analysis

最有意义的组件对照是去掉 KG retrieval、去掉 LLM iterative generation 或去掉最后的 logical validation。缺 KG 时候选更依赖模型先验，缺 LLM 时表达能力和特征多样性下降，缺 validation 时容易出现语义漂亮但逻辑不成立的特征。误差还来自数据集字段描述过短、KG 覆盖不足、实体链接错误以及 LLM 生成与真实列分布不匹配；因此“有解释”不等于“特征在统计上有用”。

## Limitations

方法依赖外部 KG 的领域覆盖、实体链接和规则质量，不同领域需要重新配置 ontology；LLM 的调用和迭代也增加成本。公开摘要和审稿版本强调了方向与 pipeline，但对所有数据集、随机种子、人工解释评价的细节并不充分，不能把结果泛化为任意 tabular domain 的保证。

## 两句话总结

ReaGen 用知识图谱做检索与逻辑约束，用 LLM 生成具有人类语义的候选特征和 utility explanation。它把自动特征工程从“语言模型自由发挥”变成“生成-验证闭环”，但效果依赖知识库覆盖和严格的数据集级实验协议。

## Evidence note

本笔记核对 OpenReview 的摘要、Introduction、Method 和公开实验描述；对无法从公开版本逐项复核的数值明确保留证据边界。

