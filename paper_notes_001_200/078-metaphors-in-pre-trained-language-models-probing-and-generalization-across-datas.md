# 078. Metaphors in Pre-Trained Language Models: Probing and Generalization Across Datasets and Languages

- **作者 / venue**：ACL 2022
- **论文**：[ACL Anthology](https://aclanthology.org/2022.acl-long.144/)

## Introduction / Method

隐喻把新概念连接到熟悉 domain，可能成为 PLM 中的语言学/认知知识。论文用 probing 和 minimum description length 分析 metaphor information 是否真的编码在表示中，并评估跨数据集、跨语言泛化。

## Baseline 与对比

使用 LCC、TroFi 等四个 metaphor detection datasets，比较 supervised probe、不同 layer、in-domain 与 out-of-domain、英语与其他语言。MDL probe 用来减少高容量 classifier 仅学习数据模式的风险。

## Findings

表示中确实存在可读的 metaphoricity information，但跨数据集泛化明显低于同分布测试；probe 可能学习 task-specific cues 而不是语言学隐喻知识。俄语等设置的 out-of-distribution 表现相对较好，说明预训练语料和目标域距离会影响迁移。

## 局限

probe accuracy 不能证明模型生成时使用隐喻表示；跨语言数据质量和标注定义不同。需要 causal intervention 和更强控制。

**一句话评价**：论文把“PLM 编码了隐喻”拆成 in-domain 可读、跨域泛化和真正使用三个不同问题。
