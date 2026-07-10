# 265. Enhancing Self-Explanation in Student Learning Through Large Language Models

- **Authors:** Jessica Wen, Angela Zavaleta Bernuy, Naaz Sibia, Andrew Petersen, Michael Liut
- **Venue / year:** ITiCSE 2025
- **Paper:** https://doi.org/10.1145/3724389.3730790
- **Tags:** Education, Self-Explanation, Human-AI Interaction, Learning Support

## Introduction

Self-explanation 能促使学生把知识重新组织并暴露理解空缺，但学生常常不知道如何开始，也难以判断自己的 explanation 是否完整。论文研究 LLM 是否能在 flipped computer organization course 中 scaffold 学生自我解释，同时避免把学生的思考直接替换成模型答案。

## Method / Framework

研究采用 A/B 设计。control 组使用固定 prompt，把自己的 explanation 与 expert explanation 对照；treatment 组与 LLM 进行交互对话，由模型帮助发现 explanation 中的 gaps。两种条件都保留学生先生成解释这一核心活动，差别是“静态比较”还是“对话式诊断与反馈”。

## Baselines / Comparisons

主要 baseline 是 fixed-prompt expert comparison，实验条件是 interactive LLM dialogue；比较 explanation quality、学生主观 comfort/value 和不同群体体验，而不是 LLM 的语言生成分数。论文重点是学习工作流效果，因此不应把 LLM response 的流畅度当作学生理解提升的替代指标。

## Experiments / Findings

公开摘要与研究记录显示，两组最终 explanation quality 没有显著差异；也就是说，对话式 LLM 并未在总体质量上明确击败固定 expert comparison。但非英语母语学生和女性学生在 LLM 条件下报告了更高的 comfort 与 perceived value，提示 LLM 的主要收益可能是降低表达门槛、提供即时低压力反馈，而不是直接提高答案质量。

## Ablation / Error Analysis

A/B 条件本身是关键消融：interactive dialogue 没有转化为总体质量显著提升，可能因为学生没有充分采纳反馈、expert comparison 已经很强，或质量指标未捕捉反思深度。还需区分“解释写得更好”“学生理解更深”和“学生更愿意参与”；LLM 也可能给出过度引导、正确但不属于学生自己的 explanation。

## Limitations

课程、学科和样本群体有限，长期保留效果、知识迁移和最终考试表现尚不清楚；主观 comfort/value 不能替代 learning gain。公开记录没有提供完整样本量、prompt、评分量表和所有统计表，因此本笔记不编造具体百分点。

## 两句话总结

该研究把 LLM 放在学生自我解释之后，用交互对话帮助学生发现知识空缺，并与固定 expert comparison 做 A/B 对照。总体解释质量没有显著提高，但弱势语言群体和女性学生感到更舒适、更有价值，说明 LLM 可能先改善参与和反馈体验，再间接影响学习。

## Evidence note

本笔记核对 ITiCSE 公开摘要、出版记录与研究摘要；当前未获得正文实验表格，因此已明确标注统计细节未复核。

