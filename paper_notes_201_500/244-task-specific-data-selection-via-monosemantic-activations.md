# 244. Task-Specific Data Selection for Instruction Tuning via Monosemantic Neuronal Activations

- **Authors:** Da Ma, Gonghu Shang, Zhi Chen, Libo Qin, Yijie Luo, Lei Pan, Shuai Fan, Lu Chen, Kai Yu
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2503.15573
- **Tags:** Instruction Tuning, Data Selection, SAE, Monosemantic Neurons

## Introduction

instruction tuning 数据很多，但对目标任务有用的样本选择困难；influence 方法不稳定，distribution alignment 又依赖表面 representation，且 neuron polysemanticity 会混淆“样本涵盖了什么能力”。MONA 用 sparse autoencoder 得到 model-centric monosemantic activation，按任务相关语义选择训练数据。

## Method / Framework

对 general instruction samples 抽取 SAE sparse monosemantic neuronal activations，定义适合稀疏 activation space 的 similarity metric，再按 target task 与样本表示的相关性选固定比例数据。选出的数据用于 instruction fine-tuning，比较不同 backbone、instruction source、selection ratio，并用 LLM data analyst 做模型无关质量检查。

## Baselines / Comparisons

在 LLaMA3.1-8B 等 backbone、OpenHermes-2.5/Less 数据上，与 random、embedding/distribution alignment、influence-based 和其他 data selection 方法比较；固定 5% selection ratio，任务包括 GSM8K、BBH、GPQA 等 general reasoning benchmark，并测试更大模型与不同数据比例。

## Experiments / Findings

- MONA 在几乎所有 target task 上达到 best/second-best；在 OpenHermes-2.5 上 GSM8K、BBH、GPQA 取得最高分，整体平均甚至超过 full-data fine-tuning。
- SAE activation 比 dense/textual similarity 更能表示 task-relevant internal computation，减少选择与目标任务无关的 instruction；selection ratio 变化也显示较少但语义对齐的数据可能优于全量噪声。
- monosemantic representation 让数据选择过程更可解释：可以检查哪些能力 feature 导致样本入选，而不是只看黑盒 influence score。

## Ablation / Error Analysis

消融 SAE layer、feature aggregation、similarity metric、selection ratio 和 backbone。polysemantic feature 会把无关能力混在一起，SAE dictionary/解释误差会导致漏选；过少样本可能覆盖不足，过多样本又稀释 task signal。

## Limitations

方法需要白盒 activation、SAE 训练和目标任务 representation，部署成本高于纯文本 embedding；选择的高分样本也可能过拟合 benchmark。monosemantic label 仍是 feature interpretation proxy，不能保证 fine-tuning 后内部能力按预期改变。

## 两句话总结

MONA 用 SAE monosemantic activation 表示 instruction 样本，再按模型内部 task relevance 选数据，解决随机/表面分布选择不稳的问题。它在多个 reasoning task 上以少量数据超过强 baseline，但 SAE 成本、feature 误差和 benchmark 过拟合仍需评估。
