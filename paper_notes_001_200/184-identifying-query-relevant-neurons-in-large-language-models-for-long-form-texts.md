# 184. Identifying Query-Relevant Neurons in Large Language Models for Long-Form Texts

- **Authors:** Lihu Chen, Adam Dejl, Francesca Toni
- **Venue:** AAAI 2025
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/34529
- **Tags:** Neuron Attribution, Knowledge Editing, Long-form, QRNCA

## Introduction

knowledge neuron / ROME 主要定位小模型和单 token triplet fact，难以处理 decoder-only Llama / Mistral 的 long-form answer。QRNCA 要找与 query 相关的 neuron clusters，并检验它们是否按 subject domain 形成局部区域。

## Method / Framework

Query-Relevant Neuron Cluster Attribution 用 multi-choice QA proxy 代替单一 fill-in-the-blank，通过 query condition 的 activation / attribution 找 neuron cluster，再用于 long-form knowledge location、editing 和 neuron-based prediction。

## Baselines / Comparisons

与 Knowledge Neuron、ROME、Knowledge Subnetwork 等 fact localization / editing 方法比较，覆盖不同模型规模、层、subject domain 和语言。指标包括 query answer accuracy、neuron localization、editing efficacy 和 general ability retention。

## Experiments / Findings

- QRNCA 能处理长回答和 decoder-only LLM，优于只针对 single-token fact 的 baseline。
- neuron distribution 出现 subject-specific localized regions，说明知识可能在模型中有主题局部化。
- 找到的 cluster 可用于知识编辑和基于 neuron 的 prediction，提供从定位到干预的应用路径。

## Ablation / Error Analysis

消融 proxy QA、query wording、cluster size、layer aggregation 和 multi-choice / free-form 对照。query 偏置、选项泄漏、长文本截断和多个主题共享 neuron 会影响归因。

## Limitations

multi-choice proxy 不完全等价于自由生成；localized region 是统计聚集，不证明单个 neuron 独立存储事实。编辑可能造成邻近知识损伤，且大模型 attribution 成本高。

## 两句话总结

QRNCA 将 knowledge-neuron 定位从短 triplet 扩展到 decoder-only LLM 的 query-relevant neuron clusters 和长文本。它发现主题局部区域并支持编辑，但 proxy task 和 cluster 因果性仍需进一步验证。
