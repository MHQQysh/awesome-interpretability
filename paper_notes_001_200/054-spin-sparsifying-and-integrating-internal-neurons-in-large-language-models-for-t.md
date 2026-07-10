# 054. SPIN: Sparsifying and Integrating Internal Neurons in Large Language Models for Text Classification

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.277/)

## Introduction 与方法

传统分类通常只用 LLM 最后一层 hidden state，忽略中间层中已经形成的 task-specific neuron。SPIN 是 model-agnostic plug-and-play 框架：从中间层选择稀疏、可解释的 internal neurons，再把它们整合成分类特征。

## Baseline 与对比

比较 final-layer classifier、encoder/decoder conventional fine-tuning、DistilBERT 等基础模型，以及 SPIN 与 fine-tuning/SoTA 方法。核心 ablation 是只用终层 vs 加入 sparsified internal representations。

## Findings

SPIN 在多个 text classification 数据集上能用冻结或轻量训练的内部特征取得竞争性甚至更高性能。稀疏选择减少冗余，并让分类依据更容易追踪到 layer/neuron；但性能提升不等于每个选中的 neuron 都具有人类可读语义。

## 局限

内部特征选择依赖阈值和任务；稀疏性可能牺牲信息，跨模型 neuron index 不可直接复用。还需要 intervention 验证选中 neurons 是否真正因果。

**一句话评价**：SPIN 把中间层 neuron 从“被忽略的内部状态”变成分类特征，兼顾参数效率和一定解释性。
