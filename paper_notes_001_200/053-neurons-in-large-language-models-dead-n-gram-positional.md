# 053. Neurons in Large Language Models: Dead, N-gram, Positional

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.75/)

## Introduction 与方法

论文以 OPT 家族不同规模模型为对象，研究单个 neuron 的可解释模式：从 never-active 的 dead neurons，到检测 token/n-gram 的 detectors，再到与文本内容无关的 positional neurons。分析尽量轻量，适合单 GPU，重点观察规模变化和层位置。

## Baseline 与对比

比较 OPT-125M 到更大模型、不同层阶段，以及去掉 positional encoding 的模型。n-gram detector 的触发数量、位置模式和模型规模是主要对照。

## Findings

- 一部分 neuron 几乎不激活，dead neuron 比例和位置随模型/层变化。
- 一些 neuron 对 token 和 n-gram 有明确触发模式，早期层更偏浅层 lexical detection，后续层可形成更长 n-gram。
- 部分 neuron 编码 position，而不依赖具体文本内容；规模扩大后 position pattern 也会改变。
- 这说明 neuron 可解释性不是单一“语义概念”，还包括稀疏性、局部模式和架构位置。

## 局限

命名模式来自激活统计，不等于证明 neuron 对最终任务有因果作用；不同实现细节（如 LayerNorm 位置）也会显著改变结果。

**一句话评价**：该工作提供了大模型 neuron 类型的细粒度地图，为 SAE 和机制电路研究提供了低成本 sanity check。
