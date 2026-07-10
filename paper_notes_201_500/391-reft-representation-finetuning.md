# 391. ReFT: Representation Finetuning for Language Models

- **Authors:** Zhengxuan Wu, Aryaman Arora, Zheng Wang, Atticus Geiger, Dan Jurafsky, Christopher D. Manning, Christopher Potts
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2404.03592
- **Tags:** `representation` `causal-intervention` `parameter-efficient` `LoReFT` `mechanistic-interpretability`

## Introduction
LoRA、adapter 和 prefix tuning 通过更新少量权重适配大模型，但它们仍把信息写回参数，难以直接解释编辑发生在哪里。ReFT 提出另一种 PEFT 视角：冻结 base model，只学习在 forward pass 的 hidden representation 上施加的 task-specific intervention。这样参数更少，也把适配动作显式放在可观测的表示路径上。

## Method / Framework
ReFT 将 representation intervention 统一成一个 family。给定某层/某些 token 的 hidden state，干预函数可以把状态投影到一个低维子空间、施加方向性偏移，再送回冻结模型。

- **LoReFT:** Low-rank Linear Subspace ReFT，在隐藏表示的低秩子空间中学习编辑，并带有正交/子空间约束；公式上用 `R` 投影表示和 learned source `Wh+b`。
- **DiReFT:** 去掉正交约束的高效消融，类似直接在 hidden representation 上施加 LoRA 风格低秩变换，效率更高但表现通常略差。
- 训练只优化干预参数，推理时在固定 token positions/layers 施加 intervention；额外开销不随 prompt 长度线性增加，因为编辑位置数固定。

## Baselines / Comparisons
论文在 commonsense、arithmetic、instruction-following 和 GLUE/NLU 四类、20 多个数据集上比较 prefix-tuning、adapter-tuning、LoRA、DoRA、full fine-tuning 和其他 PEFT。模型包括 LLaMA-1/2、Llama-3 以及 RoBERTa-base/large。

比较指标同时看任务 accuracy、可训练参数比例和推理/训练效率。LoReFT/DiReFT 的核心对照是同样任务上的 LoRA/DoRA，而不是与一个小模型比较。

## Experiments / Findings
LoReFT 通常只需约 0.025%-0.031% 的参数，在 commonsense 表中优于或接近 DoRA；Llama-3 8B 的 LoReFT 平均约 86.6，DiReFT 约 85.4，LoRA/DoRA 更低。GLUE 上 RoBERTa-large 的 LoReFT 平均约 88.2，RoBERTa-base 约 84.2，说明 hidden intervention 不只适合大 decoder-only 模型。

论文总结 LoReFT 比 LoRA 约 15x-65x 更参数高效，并在 commonsense、instruction tuning 和部分 NLU 上取得 state-of-the-art 或竞争性结果。instruction tuning 中即使把数据量减到 1/64，LoReFT 仍保持可用表现，表明干预不是简单记忆全部训练样本。

## Ablation / Error Analysis
DiReFT 是最关键消融：移除 LoReFT 的正交约束后参数/计算更简单，但在各任务常略差。算术推理是明显弱点，LoReFT/DiReFT 不如 LoRA 和 adapter，只优于 prefix-tuning，暗示需要跨多步 CoT 的计算不能仅靠固定位置的低秩修正解决。

作者还分析编辑层、位置数、rank 和超参数；过少位置表达能力不足，过多会增加成本。机制上，LoReFT 的成功说明编辑 distributed subspace 比孤立神经元更合适，但干预本身可能创造新的 causal effects，不能自动说明 base model 原来如何完成任务。

## Limitations
hidden intervention 的语义方向和层选择仍需调参，当前实验集中在文本模型，视觉语言模型和复杂多任务编辑尚未充分验证。参数高效不等于完全零推理开销；不同 PEFT 的调参预算也会影响公平性。ReFT 是可干预接口，但要把它转成机制解释仍需要 activation patching 和因果抽象分析。

## 两句话总结
ReFT 冻结大模型，只在 hidden representations 上学习低秩干预，LoReFT 因此以极少参数获得了比 LoRA 更好的综合效率和多任务表现。它把适配动作放到可观测表示路径上，为机制解释和行为控制提供了连接，但干预方向本身未必等于模型原有因果电路。

## Evidence note
已读取 arXiv 2404.03592 v3 本地 PDF；数字来自摘要、LoReFT/DiReFT 定义、表 1-4 及算术/机制讨论。
