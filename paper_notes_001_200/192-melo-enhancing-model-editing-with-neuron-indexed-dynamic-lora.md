# 192. MELO: Enhancing Model Editing with Neuron-Indexed Dynamic LoRA

- **Authors:** Lang Yu, Qin Chen, Jie Zhou, Liang He
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/29916
- **Tags:** Model Editing, Dynamic LoRA, Neuron Index, Locality

## Introduction

模型编辑要求小成本地修改事实、分类规则或幻觉，同时保持未编辑知识不变。普通 LoRA 会把多个编辑混在同一组低秩参数中，容易产生干扰；MEND、SERAC、ROME、GRACE 等方法又分别在训练数据、额外模块、优化成本或编辑范围上有取舍。MELO 的问题是能否让每条编辑只激活自己的局部参数，并支持连续编辑。

## Method / Framework

MELO 为每个编辑样本学习一个小 LoRA block，并把其 neuron/hidden-state key 建入 vector database。推理时用输入最后 hidden state 检索最近的 cluster 和 key，只动态激活对应 LoRA block，再与原模型输出相加；因此不同事实可以按 key 路由到不同编辑参数，而不是所有样本共享一套 adapter。训练目标同时优化编辑成功与作用范围，支持 sequential editing、generalization 和 locality。

## Baselines / Comparisons

实验覆盖 SCOTUS 文档分类、zsRE 问答和 GPT2-XL hallucination correction，backbone 分别包括 BERT、T5-Small/T5-Large 与 GPT2-XL。对比 Vanilla LoRA、MEND、SERAC、ROME、GRACE 等；指标是 Edit Success (ES)、Locality、zsRE 的 F1、SCOTUS accuracy、幻觉修正后的 perplexity，以及编辑速度和参数量。

## Experiments / Findings

- MELO 在三类编辑任务上总体优于近期方法；在多数设置的 ES 和 Locality 上比 GRACE 最高约提升 15%，并且无需额外训练数据。
- zsRE 的 holdout rephrasing 上 Generality 更好，说明检索到的是与编辑事实相关的局部表示，而不是只记住一个表面模板；NQ/WebText 上的 locality 也用于检查未编辑知识是否被误改。
- 论文报告编辑速度约为最佳 baseline 的 50 倍，原因是只训练/激活相关的小 LoRA block；这体现了“神经元索引”同时是效率机制和一定程度的可追踪编辑单元。

## Ablation / Error Analysis

关键消融包括 key layer、LoRA block 数量/秩和向量检索范围。T5-Small 在 zsRE 上用第 4 层 hidden state 作 key 的 edit success 与 locality 最好；用浅层 key 时 locality 明显下降，因为浅层更偏句法表面，难以识别编辑作用域。检索错误、相似事实碰撞和连续编辑累积会导致错误 block 激活或非目标样本被改变。

## Limitations

vector database 的索引规模会随编辑数增长，动态路由还依赖 hidden-state 相似度阈值；相似事实之间并没有天然清晰的边界。实验模型和任务仍有限，长期编辑、冲突事实、跨语言与多模态编辑尚未充分验证，且 locality 只能说明输出保持，不等于内部知识真正未变。

## 两句话总结

MELO 用 neuron-indexed key 检索并动态激活独立 LoRA block，把连续模型编辑变成按事实路由的局部干预。它在 ES、locality、generality 和速度上优于多种编辑 baseline，但索引冲突、规模增长与深层因果定位仍是主要问题。
