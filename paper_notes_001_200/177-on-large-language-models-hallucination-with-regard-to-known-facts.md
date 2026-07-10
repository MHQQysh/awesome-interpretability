# 177. On Large Language Models’ Hallucination with Regard to Known Facts

- **Authors:** Che Jiang, Biqing Qi, Xiangyu Hong, Dayuan Fu, Yang Cheng, Fandong Meng, Mo Yu, Bowen Zhou, Jie Zhou
- **Venue:** NAACL 2024
- **Paper:** https://aclanthology.org/2024.naacl-long.60
- **Tags:** Hallucination, Inference Dynamics, Known Facts, Probing

## Introduction

已有 hallucination 研究常问模型是否知道事实，却少问模型明明在参数中存有正确 fact，为什么特定 prompt 仍会输出错误答案。论文用相同 knowledge triplet 的不同问法构造 correct / hallucinated pairs，研究 inference dynamics。

## Method / Framework

给定 ground-truth fact，设计不同 prompt 让模型生成正确或错误答案；记录各层 output-token probability trajectory。用 layer-wise features 和 SVM / probes 区分正确与 hallucinated generation，并分析错误答案在什么时候偏离正确 token。

## Baselines / Comparisons

比较不同 prompt、模型层、token probability、hidden representation 和 decoding；以正确回答、错误回答、parametric knowledge probing 与最终 accuracy 为对照。

## Experiments / Findings

- 模型可能在较早层已经对正确 fact 有较高信号，但后续层或 prompt-specific inference 把概率推向错误 token。
- correct / hallucinated trajectories 可被 layer-wise classifier 区分，说明“知道事实”和“说出事实”是不同阶段。
- 问法、模板和上下文会诱发同一三元组的不同输出，不能只用静态 knowledge probe 判断 hallucination risk。
- 发现为 layer-wise intervention、early warning 和 faithfulness-aware decoding 提供依据。

## Ablation / Error Analysis

消融 prompt wording、layer、token position、fact type 和 classifier。错误常来自实体共现、关系方向、语言模板和 decoding competition；probe 能预测错误模式但不自动告诉如何修复。

## Limitations

实验事实和模型范围有限，layer probe 仍是相关性证据；SVM / probability trajectory 不能证明某个 neuron 是 hallucination cause。真实开放域、多跳事实和对话状态更复杂，干预可能损害正常生成。

## 两句话总结

论文证明 LLM 可能“内部知道正确事实”却在生成后续过程中转向错误答案，并用层间 token dynamics 识别这种分叉。它把 hallucination 从静态知识缺失推进到 inference-time failure，但还需要因果干预验证。
