# 193. Sparsity-Guided Holistic Explanation for LLMs with Interpretable Inference-Time Intervention

- **Authors:** Zhen Tan, Tianlong Chen, Zhenyu Zhang, Huan Liu
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/30160
- **Tags:** Sparse Concept Bottleneck, Inference Intervention, Concept, Subnetwork

## Introduction

注意力可视化通常是局部 token 解释，concept bottleneck model 能给出人类更容易理解的概念，却可能丢失 LLM 内部细粒度路径。论文提出要同时解释输入、子网络和概念三层，并让解释不只是事后展示，还能在推理时定位错误并干预模型。

## Method / Framework

SparseCBM 把 LLM 的预测路径组织成 token/输入特征 -> 稀疏 concept-specific subnetwork -> 概念 -> task label。作者用二阶 pruning 构造概念对应的稀疏子网络，再用 concept bottleneck 训练概念预测和最终任务；推理时可对相关稀疏 mask 做 inference-time intervention，并用分解的 joint loss 保持主任务性能、学习错误修正。

## Baselines / Comparisons

实验在 CEBaB 五分类和 IMDB-C 二分类上进行，backbone 包括 DistilBERT、BERT、RoBERTa、OPT-1.3B，并与 standard fine-tuning、普通 CBM、GPT-4 prompting 等比较。指标同时报告 task accuracy/Macro-F1 与 concept accuracy/Macro-F1，避免只因解释概念增加而掩盖任务性能下降。

## Experiments / Findings

- SparseCBM 在多数 backbone/数据集组合上保持或提高 task 与 concept 指标；例如 BERT 在 CEBaB 上 concept/task 分别为 83.5/85.6 与 66.9/79.1，在 IMDB-C 上为 69.8/65.2 与 76.5/71.6（格式为 Accuracy/Macro-F1）。
- 与 standard 模型相比，性能下降很小，同时增加了概念级和子网络级可读性；与普通 CBM 相比，稀疏结构提供了更明确的“哪些神经元/连接参与概念”的路径。
- 案例分析显示，模型可以利用概念和稀疏神经元解释误判原因，并在 inference time 对相关路径做修正，而不需要重新训练完整 LLM。

## Ablation / Error Analysis

消融关注 sparsity、concept bottleneck、joint loss 和 inference-time mask intervention。稀疏率过高会切掉分布式证据，过低又使解释回到不可读的密集网络；概念标签错误会沿 bottleneck 传播到最终分类。二阶 pruning 的重要性估计还可能把相关但非因果的神经元保留下来，所以案例解释需要与输入扰动/干预结果一起看。

## Limitations

CEBaB 和 IMDB-C 的概念集合较小且任务偏分类，不能直接代表开放式生成或长链推理。concept accuracy 不能自动证明概念是模型真实使用的原因，inference-time intervention 也可能改变分布外行为；稀疏 mask 的训练和存储成本在更大 LLM 上仍需评估。

## 两句话总结

SparseCBM 用稀疏子网络把 LLM 的输入、概念、神经元路径和最终预测串成一条可读、可干预的解释链。它在两个真实文本数据集上以很小任务代价增加概念解释能力，但 concept bottleneck 与稀疏归因仍需因果验证和更大生成模型测试。
