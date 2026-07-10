# 167. Finding Skill Neurons in Pre-trained Transformer-based Language Models

- **Authors:** Xiaozhi Wang, Kaiyue Wen, Zhengyan Zhang, Lei Hou, Zhiyuan Liu, Juanzi Li
- **Venue:** EMNLP 2022
- **Paper:** https://aclanthology.org/2022.emnlp-main.765
- **Tags:** Neuron Probing, Skills, Pruning, Transferability

## Introduction

预训练 Transformer 的 task skill 如何分布在 neuron 中并不清楚。作者观察到 prompt tuning 后，某些 neuron 在正负标签样本上 activation 分布明显不同，于是问这些 skill neurons 是否真的支撑任务、是否 task-specific，以及能否用于 pruning 和 transfer prediction。

## Method / Framework

对 binary task，计算 soft prompt token 上每个 neuron 的训练集平均 activation 作为 baseline；样本 activation 高于 baseline 视为预测一个 label，聚合 validation prediction 得到 predictivity score，top neurons 定义为 skill neurons。多分类拆成多个 binary subtasks 后合并。

干预时只保留或扰动 skill neurons，比较 prompt tuning、adapter tuning、BitFit 和全量 fine-tuning。还用 skill neurons 做 network pruning，并用 task 间 neuron overlap 作为 transferability indicator。

## Baselines / Comparisons

与全模型、随机 neuron、bottom / top activation neuron、普通 prompt tuning、adapter 和 BitFit 比较。任务包括 SST-2 等分类任务，指标是任务 accuracy、perturbation drop、参数比例和 inference speedup。

## Experiments / Findings

- 扰动 skill neurons 会显著降低对应任务性能，支持它们不是普通高激活 neuron，而是任务相关计算单元。
- 相似任务的 skill neuron 分布更相似，说明 neuron overlap 可用于判断 prompt / task transferability。
- skill neurons 主要在 pretraining 中形成：prompt tuning 找到的 neurons 在冻结 neuron weights 的 adapter / BitFit 中仍然重要。
- 只保持 top skill neurons active，可把 Transformer 缩到约 66.6% 原始参数并获得约 1.4 倍 inference speedup。

## Ablation / Error Analysis

消融 top-k 数量、activation threshold、prompt、task similarity 和 perturbation strategy。保留过少会丢失通用语言能力，保留过多则速度收益小；某些相似任务共享 neuron，但相同 label 形式不代表相同 skill。activation correlation 也可能反映数据偏差而非真正因果能力。

## Limitations

实验主要是 encoder / classification 和 soft prompt，不能直接推广到生成式 LLM 的长推理。neuron perturbation 说明必要性但不说明唯一性，多个 neuron 可能冗余。pruning 的速度收益受硬件和稀疏 kernel 支持影响，不能只按参数比例估算。

## 两句话总结

论文通过 prompt-tuning activation 找到 task-specific skill neurons，并用扰动实验验证它们对任务性能具有因果影响。skill neuron 可用于解释、剪枝和迁移判断，但任务范围、神经元冗余和真实硬件收益仍需进一步验证。
