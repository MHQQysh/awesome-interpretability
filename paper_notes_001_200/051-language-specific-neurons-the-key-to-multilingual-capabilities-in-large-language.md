# 051. Language-Specific Neurons: The Key to Multilingual Capabilities in Large Language Models

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.309/)

## Introduction 与方法

多语言 LLM 的语言能力可能由共享 neurons 和 language-specific neurons 共同实现。论文提出 language activation probability 等指标，从 LLaMA-2 的 neuron activation 中定位对某一语言更敏感的区域，并在语言建模与生成中做 perturbation。

## Baseline 与对比

比较 language-specific identification 方法、随机选 neuron，以及基于 activation/probability 的不同选择策略；使用 PPL、生成质量和语言切换效果评价。

## Findings

语言特定 neuron 的激活概率在对应语言上更高，去激活这些区域会提高目标语言 PPL 或损伤生成；随机 neuron 的影响较小。结果说明 multilingual ability 不是完全均匀分布在参数中，但 neuron 只是局部接口，不等于完整语言 circuit。

## 局限

语言、tokenizer、阈值和生成 prompt 会改变定位结果；语言特定神经元之间可能重叠，过度编辑会伤害共享能力。

**一句话评价**：论文将多语言能力拆成可检测的 language-specific activation patterns，为语言选择性编辑提供了依据。
