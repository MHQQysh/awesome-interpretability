# 182. Few-Shot Self-Rationalization with Natural Language Prompts

- **Authors:** Ana Marasovic, Iz Beltagy, Doug Downey, Matthew E. Peters
- **Venue:** NAACL Findings 2022
- **Paper:** https://aclanthology.org/2022.findings-naacl.31
- **Tags:** Self-Rationalization, Few-shot, Rationale, Plausibility

## Introduction

self-rationalization 模型要同时预测 label 和生成 free-text explanation，但过去依赖大量逐样本人工 rationale，成本阻碍推广。论文提出 FEB benchmark 和 prompt-based few-shot setting，检验少量 examples 是否足够。

## Method / Framework

FEB 统一四个英文 self-rationalization datasets、输入输出格式与 explanation metrics。作者系统搜索 natural-language prompt、example formatting、label / rationale order 和模型规模，比较 zero-shot、few-shot 与不同 demonstration 组合。

## Baselines / Comparisons

比较 human rationale、GPT-3 生成 rationale、不同 prompt、不同 model size、只预测 label 与 self-rationalization。指标包括 task performance、explanation plausibility、faithfulness / quality 和人类偏好。

## Experiments / Findings

- few-shot prompting 可以让模型同时生成 label 与 rationale，但收益依赖 prompt 和模型规模。
- GPT-3 生成 explanation 的平均 human plausibility 最高约 51%，人类 explanation 约 76%，说明流畅文本仍有明显质量差距。
- larger model 和更合适的 natural-language prompt 能提高 rationale quality，但离人类解释仍有空间。
- FEB 提供跨数据集可比的评测，暴露 explanation generation 与 label accuracy 不同步。

## Ablation / Error Analysis

消融 prompt wording、demonstration 数量、任务数据集和模型规模。错误包括理由不支持 label、重复 rubric、遗漏 commonsense bridge 和把输入表面词改写成解释。

## Limitations

四个数据集和英文任务有限；plausibility 由人类判断，faithfulness 的 gold rationale 不一定唯一。few-shot prompt 对 context window 和示例顺序敏感，不能解决大规模 rationale 标注需求。

## 两句话总结

FEB 让 few-shot self-rationalization 有了统一 benchmark，并证明少量自然语言 prompt 可以启动同时预测和解释。结果仍显示 GPT-3 rationale 的人类可接受度明显低于人工解释，不能把生成流畅当作 faithful。
