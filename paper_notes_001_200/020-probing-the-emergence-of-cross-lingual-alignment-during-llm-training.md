# 020. Probing the Emergence of Cross-lingual Alignment during LLM Training

- **作者 / venue**：Jiaoda Li, Yuxuan Zhang, Shujian Huang, Jiajun Chen；Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.724/)
- **任务**：训练动态中的跨语言对齐；分析不同语言是否逐渐共享同一组神经元子网络

## 1. Introduction：从 transfer accuracy 回到内部动力学

多语言 LM 能在没有平行句监督时进行 zero-shot transfer。常见解释是共享 subword、共享参数或数据共现，但这些解释仍然停留在架构和输入层面。作者提出一个更内部的问题：不同语言中对应的词汇/语法特征，是否会在训练过程中逐渐激活同一组 neurons？如果会，alignment 在什么时候出现、出现在哪些层？

论文把 cross-lingual alignment 定义为可测量的神经元重叠和相关性，而不是只看最终下游准确率。

## 2. Method：checkpoint probing + neuron overlap

- **模型**：BLOOM-560M、BLOOM-1B1、BLOOM-1B7 的多个中间 checkpoint。
- **语言学特征**：从多语言数据中构造词-标签对，覆盖 POS、number、gender、tense 等 morphosyntactic features。
- **Intrinsic probing**：在每个 checkpoint、每个 layer 上训练 probe，测量特征是否可读出。
- **Neuron selection**：对每个语言/feature 选取 top-k expert neurons，再计算不同语言之间的 overlap 和相关性。
- **Transfer correlation**：将神经元对齐程度与跨语言下游 transfer 表现联系起来，分析共享是否真的具有功能意义。

## 3. Baseline 与对比

- **训练时间轴 baseline**：同一模型在早期、中期、后期 checkpoint 的差异，是研究的主要对照。
- **模型规模 baseline**：BLOOM-560M、1B1、1B7，测试规模是否改变 alignment 形成速度。
- **不同语言/特征对比**：语言相似度、script、数据规模和 morphosyntactic category 的差异。
- **下游 transfer 对比**：将 neuron overlap 与语法、语义任务表现相关联，避免把统计重叠直接当作有效对齐。

## 4. Findings：alignment 如何出现

- 训练早期，特征更多由语言特定神经元编码；随着预训练进行，不同语言对应特征的 neuron overlap 增加，说明模型逐步复用共享子网络。
- 对齐并不是所有层同步发生：部分中间层更容易表现跨语言共享，而输入/输出相关层保留更多语言特定信息。
- 共享神经元越多，相关语言的 cross-lingual transfer 往往越好，但 overlap 不是充分条件；语言家族、训练数据和特征类型都会改变相关性。
- 论文强调，alignment 的形成是逐步的，不是训练到某个点突然“涌现”；这对解释所谓 emergent multilingual ability 很重要。

## 5. Ablation / 机制解释

- 改变 checkpoint 能区分静态 neuron overlap 与训练中逐渐形成的 overlap。
- 改变模型规模能检验大模型是否只是拥有更多共享容量，还是确实形成更强的跨语言抽象。
- 按特征类型分解可以看到，POS/形态等低层结构与语义概念的对齐路径不同。
- 只激活被选出的 neurons 不能完整重建语言能力，说明单个 neuron overlap 更像局部证据，需要放在 circuit/层级上下文中解释。

## 6. 局限

只分析 BLOOM 家族和有限语言；linear probe 仍可能把不可用于生成的可读信息读出来；neuron overlap 对 top-k、阈值和随机种子敏感。跨架构、跨 tokenizer 和更大规模模型的结论仍需复现。

**一句话评价**：论文把跨语言泛化从最终 transfer score 追溯到 checkpoint 级神经元重叠，说明 alignment 更像训练中逐步形成的共享子网络，而不是简单的 token overlap。
