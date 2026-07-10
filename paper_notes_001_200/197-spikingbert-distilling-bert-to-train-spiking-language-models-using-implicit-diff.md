# 197. SpikingBERT: Distilling BERT to Train Spiking Language Models Using Implicit Differentiation

- **Authors:** Malyaban Bal, Abhronil Sengupta
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/28975
- **Tags:** Efficient LLM, Spiking Language Model, Knowledge Distillation, Energy

## Introduction

Transformer language models的计算与能耗来自密集的乘加和深层反向传播，而生物神经系统用稀疏 spike 事件传递信息。论文要回答的是：能否保留 BERT 的语言任务能力，同时训练一个可扩展的 spiking language model，并且不依赖传统 BPTT 带来的巨大内存开销。

## Method / Framework

SpikingBERT 用 LIF neuron、spiking attention 和 equilibrium average spiking rate (ASR) 表示中间状态。作者证明隐式网络达到 equilibrium 后可用 implicit differentiation 求梯度，绕开 spike 不可导与长时间展开；随后用两阶段 ANN-SNN knowledge distillation，先从通用 BERT/ Wikipedia 蒸馏，再从任务微调 BERT 的中间层和预测层蒸馏给 spiking student。

## Baselines / Comparisons

在 GLUE 的 QQP、MNLI-m、SST-2、QNLI、RTE、MRPC、STS-B 上比较标准 BERT、TinyBERT、其他 efficient LM，以及 SpikeGPT 等 spiking 模型。指标是各任务 accuracy、MRPC 的 accuracy/F1、STS-B 的 Pearson/Spearman；还比较有无 general/task-based KD、时间步数和阈值对 accuracy、energy 的影响。

## Experiments / Findings

- SpikingBERT4 在 GLUE 上能达到接近甚至超过部分高效 BERT 的结果，例如表中 QQP 86.82、MNLI-m 78.10、SST-2 88.19、QNLI 85.20、RTE 66.06，并在较小参数量下与 TinyBERT 等对比。
- 去掉提出的 KD 后，各数据集 accuracy 至少下降约 4%--5%，说明 equilibrium 状态本身不足以替代教师信息，内部层蒸馏是性能关键。
- 在 SST-2 的能耗-精度权衡中，约 16 个 convolution time steps 能在接近最高 accuracy 的同时达到近两倍于同规模非脉冲模型的 energy efficiency；ACC 操作按 45nm 估计约 0.9pJ，低于 MAC 的 4.6pJ。

## Ablation / Error Analysis

消融覆盖 general KD、task-based internal KD、spiking attention、time steps 和 threshold。没有 KD 的直接标签训练会掉点；time steps 太少会损失精度，太多又削弱能耗优势；提高 threshold 可降低 ASR 和功耗，但可能让稀疏 spike 过度，导致信息丢失。不同任务的 layer grouping 也会影响蒸馏效果。

## Limitations

能耗主要是基于硬件操作估算，不是完整 Loihi 等神经形态芯片上的端到端实测；spike 模型的延迟、batching 和内存访问也可能抵消部分 ACC 优势。实验规模仍接近 BERT 而非现代超大 LLM，且高质量 KD 需要教师模型和额外训练阶段。

## 两句话总结

SpikingBERT 用 equilibrium spiking rate、implicit differentiation 和 ANN-SNN KD，把 BERT 的语言知识迁移到稀疏脉冲 Transformer，并降低训练内存与推理能耗。GLUE 结果和消融支持 KD 的必要性，但能耗收益仍依赖时间步、硬件实现和估算假设。
