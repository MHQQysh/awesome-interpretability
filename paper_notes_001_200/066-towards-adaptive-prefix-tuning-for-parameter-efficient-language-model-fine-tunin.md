# 066. Towards Adaptive Prefix Tuning for Parameter-Efficient Language Model Fine-tuning

- **作者 / venue**：ACL Short 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-short.107/)

## Introduction / Method

固定 prefix tuning 为每层提供相同形式的可训练 prefix，但不同层对任务的需求不同。APT 根据前一层 hidden state 生成 adaptive prefix/scale，使 prefix 随输入和层变化，同时保持 base LM 参数冻结。

## Baseline

比较 vanilla fine-tuning、P-Tuning v2（PT-2）、variable/enlarged prefix、以及 APT；在 SuperGLUE 和 NER 上报告 accuracy/micro-F1，并使用 BERT-large、RoBERTa-large、DeBERTa-xlarge 等 backbone。

## Findings

APT 在多项任务上优于固定 prefix，部分设置平均提升约 0.8%；adaptive prefix 的动态性让不同层获得不同任务条件。它以较少参数达到接近或超过 full fine-tuning 的效果，但性能依赖 prefix length、学习率和 layer adapter。

## 局限

参数效率不等于可解释：adaptive prefix 是输入条件表示，仍需分析它改变了哪些层和 token。不同模型的 prompt/prefix 结构不可直接比较。

**一句话评价**：APT 展示了层条件的 PEFT 设计，说明“冻结模型”并不意味着内部变化消失，而是把适配集中到可分析的 prefix 路径。
