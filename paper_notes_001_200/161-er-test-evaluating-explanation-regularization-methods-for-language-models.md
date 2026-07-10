# 161. ER-Test: Evaluating Explanation Regularization Methods for Language Models

- **Authors:** Brihi Joshi, Aaron Chan, Ziyi Liu, Shaoliang Nie, Maziar Sanjabi, Hamed Firooz, Xiang Ren
- **Venue:** EMNLP Findings 2022
- **Paper:** https://aclanthology.org/2022.findings-emnlp.242
- **Tags:** Explanation Regularization, Faithfulness, OOD, Rationale

## Introduction

Explanation regularization（ER）让模型的 machine rationale 与人类认为重要的 token 对齐，希望模型学到更可靠的决策路径。过去主要报告 in-distribution accuracy，无法说明这种对齐是否改善分布外泛化，也缺少统一比较不同 rationale loss、annotation 类型和标注预算的协议。

## Method / Framework

ER-Test 从 unseen datasets、contrast sets、functional tests 三个维度测 OOD。训练时以 token attribution 为 machine rationale，用 token-level human rationale 或 task-level prior 作为约束；比较 attention / gradient 类 rationale 与 human mask 的 alignment loss。实验覆盖两个任务、六个数据集，并系统改变 rationale 数量、类型、选择和 annotation time budget。

## Baselines / Comparisons

与普通 supervised model、label-only training、instance-level rationale、task-level rationale、不同 rationale alignment criterion 的 ER 模型比较。指标分 ID accuracy、OOD accuracy、contrast-set robustness 和功能测试，避免只按训练集或同分布测试判断解释正当性。

## Experiments / Findings

- ER 对 ID performance 通常影响很小，却能带来明显 OOD gains。
- 最好的 alignment criterion 依赖任务，没有统一最优 loss。
- task-level rationale 或少量 human rationale 也能改善 OOD，不一定需要逐样本昂贵标注。
- rationale annotation 相比额外 label annotation 更节省时间，却能提供更好的 OOD 改善。
- 结果说明 explanation regularization 的价值主要体现在 shortcut 和分布变化下，而不是继续挤压已饱和的 ID accuracy。

## Ablation / Error Analysis

消融 instance-level / task-level、annotation 数量、rationale 选择、loss、训练时间和 contrast set。过少或偏置 rationale 会把模型引向错误证据；不同任务的最佳 alignment 变化大。功能测试还显示 accuracy 提升可能只来自某类合成扰动，必须多维评估。

## Limitations

human rationale 也可能不完整或不是模型真正需要的证据；token attribution 本身有 faithfulness 问题。实验任务和模型规模有限，OOD 结果不能直接推广到生成式 LLM。ER 还可能压制模型发现非人类但有效特征的能力。

## 两句话总结

ER-Test 证明解释对齐对分布外泛化比对同分布准确率更有价值，并给出 unseen、contrast 和 functional 三维评测。它也说明标注预算和 loss 设计必须按任务选择，不能把 rationale alignment 当成统一处方。
