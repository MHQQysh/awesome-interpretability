# 413. Identification of Knowledge Neurons in Protein Language Models

- **Authors:** Divya Nori, Shivali Singireddy, Marina Ten Have
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2312.10770
- **Tags:** `protein-language-model` `knowledge-neurons` `attribution` `interpretability`

## Introduction
蛋白质语言模型如 ESM 可以从氨基酸序列预测蛋白性质、结构和功能，但生物医学用户需要知道哪些内部单元支持这些预测。本文在 enzyme sequence classification 上识别“knowledge neurons”，比较 activation-based 和 integrated-gradient-based neuron selection。

## Method / Framework
先对 ESM 做 enzyme classification fine-tuning，再根据 neuron activation 或 integrated gradients 计算重要性，保留选出的子集并重新评估任务。作者分析 self-attention 中 key-vector prediction networks 的 neuron density，试图定位表达蛋白功能信息的组件。

## Baselines / Comparisons
对比随机 neuron selection、activation-based selection、integrated-gradient selection，以及保留全部/子集 neurons 的 ESM。指标是 enzyme 分类性能和选中 neuron 的数量/密度。

## Experiments / Findings
两种知识 neuron 选择都持续优于 random baseline，说明部分 neuron 对 enzyme property prediction 的贡献集中。作者观察到 self-attention 的 key-vector networks 中知识 neuron 密度较高，支持 key/value 结构在 protein semantic retrieval 中承担重要作用。

## Ablation / Error Analysis
selection method、subset size、activation/gradient layer 和随机种子是主要消融。activation 高不一定是因果重要，gradient 可能受饱和、序列长度和类别 imbalance 影响；保留 neuron 后性能下降也可能来自组合电路被破坏，而不等于单 neuron 没有知识。

## Limitations
蛋白质功能标签和 fine-tuning 数据有限，knowledge neuron 的语义需要生物学实验验证。神经元选择属于 post-hoc attribution，未证明 ESM 的原始预训练机制如何储存知识；结果也不能直接外推到自然语言 LLM。

## 两句话总结
本文在 ESM enzyme 分类模型中用 activation 和 integrated gradients 选择 knowledge neurons，两者都比随机选择更能保留预测性能，并发现 key-vector networks 的相关 neuron 密度较高。它提供了蛋白语言模型的可审计组件线索，但选择结果仍需组合级干预和生物学验证。

## Evidence note
已读取 arXiv 2312.10770 本地 PDF 的摘要、ESM fine-tuning、两种选择方法、random baseline 和 key-vector density 分析。
