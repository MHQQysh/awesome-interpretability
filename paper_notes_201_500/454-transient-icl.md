# 454. The Transient Nature of Emergent In-Context Learning in Transformers

- **Authors:** Aaditya K. Singh, Stephanie C. Y. Chan, Ted Moskovitz, Erin Grant, Andrew M. Saxe, Felix Hill
- **Venue / year:** NeurIPS 2023
- **Paper:** [arXiv:2311.08360](https://arxiv.org/abs/2311.08360)
- **Tags:** `in-context-learning` `training-dynamics` `in-weight-learning` `circuit-competition`

## Introduction
ICL 通常被当成一旦出现就会持续的能力，但小 Transformer 的训练可能继续下降，同时 ICL 逐渐消失。论文要回答：ICL 是否只是训练中间态，模型规模、数据分布和正则化能否改变这种 transience，以及 ICL 消失是否与 in-weights learning (IWL) 电路竞争有关。

## Method / Framework
作者用 Omniglot 手写字符构造 exemplar-label 序列：训练序列同时允许匹配上下文的 ICL 和固定映射的 IWL。ICL evaluator 每次随机重映射标签，只有读取上下文才能答对；IWL evaluator 刻意不放匹配 exemplar，测试权重中记忆的映射。默认模型 12 层、64 维 embedding、17 token 序列，Adam 训练约 5e7 steps，并扩展到 LLaMA token embedding 数据。

## Baselines / Comparisons
比较 ICL/IWL 两种 evaluator 的曲线，而不是只看训练 loss；扫描层数、embedding width、类别数、每类 exemplar 数、数据 burstiness、Omniglot/LLaMA-derived 数据和 regularization。还比较 early stopping 与 L2/weight decay，以及对 MLP、ResNet 等子模块单独施加衰减。

## Experiments / Findings
在 ICL 足以完全解决训练任务的设置中，ICL 先出现、达到峰值，再随训练消失，IWL 同期上升而 train loss 继续降低。增加层数不能稳定保留 ICL；增加每类 variation 提高峰值却可能加快衰减，增加类别数通常带来更高峰值和更缓衰减。结果跨模型大小与 Omniglot/LLaMA-derived 数据成立。

L2 regularization 在测试时间尺度内最可靠地消除 ICL transience；但过强的正则化（约大于 `1e-4` 的设置）会让模型过度正则。作者的初步证据是 ICL 与 IWL 争夺残差流/参数资源：MLP 或 ResNet weight decay 能减轻 ICL 衰退，而简单早停只是在时间上截取能力峰值。

## Ablation / Error Analysis
深度、每类样本数、类别数和不同数据分布的扫描显示，ICL 峰值和 decay slope 并非由单一 scaling law 决定。部分随机初始化在 LLaMA-derived 数据上根本不出现 ICL，说明“出现后必消失”应理解为在 ICL 已出现的运行中观察到的规律；不同正则强度也存在不出现 ICL 或过拟合的边界。

## Limitations
实验是合成 few-shot 任务，不直接等价于 web-scale LLM。ICL/IWL 不是完全可分的机制，且电路竞争只是间接证据；训练步数、seed 和 evaluator 设计可能影响“峰值/衰退”的观测。

## 两句话总结
论文证明 ICL 可能是训练中的 transient capability：模型先学会读上下文，之后用更低训练损失的权重记忆方案替代它。增加数据结构能改变峰值但不能保证持久性，L2 正则是最有效的保留手段，并提示 ICL 与 IWL 电路存在竞争。

## Evidence note
已读取数据构造、模型设置、规模/数据扫描、正则化、竞争机制和附录中的 seed/正则化补充实验。
