# 474. Neuron to Graph: Interpreting Language Model Neurons at Scale

- **Authors:** Alex Foote, Neel Nanda, Esben Kran, Ioannis Konstas, Shay B. Cohen, Fazl Barez
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2305.19911](https://arxiv.org/abs/2305.19911)
- **Tags:** `neuron-interpretability` `graph-explanation` `scalable-interpretability`

## Introduction
单个 neuron 的 top-activating examples 往往只覆盖局部模式，人工归纳又无法扩展到数百万 neuron。Neuron2Graph (N2G) 的目标是从训练语料自动提取“哪些 token/context 组合促成该 neuron 激活”的图，并用真实激活验证图的预测能力。

## Method / Framework
N2G 从高激活样本出发，用 truncation 和 saliency 找到对末 token neuron activation 最相关的 token，再通过多样化样本扩充行为范围。输出是 token dependency graph，可视化为语义解释，也可反向生成 token activation prediction；图之间还能用于 neuron search 和 similarity comparison。

## Baselines / Comparisons
与两种自动 baseline 比较平均 precision/recall/F1，并与人工查看 top examples 形成 qualitative reference。实验跨模型 layer，重点验证图生成的 activation prediction，而不是只评估人类主观可读性。

## Experiments / Findings
N2G 在各层平均 precision/recall/F1 上优于两种 baseline；图能抓到 neuron 的核心触发词，并支持搜索“响应某 token 的 neuron”和比较相似 neuron。作者展示单张 Tesla T4 可为 6-layer Transformer 的全部 neuron 生成图，强调这是 research infrastructure 而非单例 demo。

## Ablation / Error Analysis
truncation 保留局部关键 token，saliency 过滤无关词，多样化样本弥补只看 top activation 的偏差；去掉这些步骤会导致图过宽、遗漏或只记住表面模板。限制部分承认训练数据覆盖不足时，图只能描述 neuron 的一部分行为。

## Limitations
图是根据 activation correlation/上下文替换构造的，不直接证明 neuron 对最终输出的因果贡献；polysemantic neuron 可能需要多个图或更高阶关系。自动图的语义仍需人工理解，模型语料和 tokenizer 限制泛化。

## 两句话总结
N2G 用 saliency、truncation 和数据扩充把 neuron 的激活行为自动表示成可搜索 graph，并可用真实 activation 做定量验证。它把 neuron interpretability 从人工 top-example 阅读推进到规模化工具，但图的完整性和因果性仍需额外干预实验。

## Evidence note
已读取 N2G 算法、三类用途、逐层 precision/recall/F1 对照、T4 scalability 和 limitations。
