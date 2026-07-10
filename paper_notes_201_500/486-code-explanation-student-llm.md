# 486. Comparing Code Explanations Created by Students and Large Language Models

- **Authors:** Juho Leinonen, Paul Denny, Stephen MacNeil, Sami Sarsa, Seth Bernstein, Joanne Kim, Andrew Tran, Arto Hellas
- **Venue / year:** ITiCSE 2023
- **Paper:** [arXiv:2304.03938](https://arxiv.org/abs/2304.03938)
- **Tags:** `code-explanation` `human-evaluation` `computing-education` `LLM-faithfulness`

## Introduction
代码解释是程序理解和写作能力的桥梁，但教师难以为大班级提供大量 exemplar。论文比较约 1000 名课程学生写的解释与 GPT/LLM 生成解释，研究准确性、可理解性、长度和学生真正重视的质量标准。

## Method / Framework
对三个函数分别收集 student explanations，并用 LLM 按相同代码生成 explanation；学生评审两类文本的 accuracy、understandability 和 length，同时回答 ideal explanation length/criteria。统计比较采用问卷评分与 Mann-Whitney U tests。

## Baselines / Comparisons
核心 baseline 是 student-authored explanations；同时对照 instructor/learnersourcing 与此前 Codex explanation 工作。比较重点是 LLM 能否成为 scalable example，而不是让 LLM 直接给学生个性化答案。

## Experiments / Findings
LLM explanations 被评为显著更易懂、也更准确地概括代码；两者在理想长度上大体相当。学生偏好的 explanation 通常明确说明 purpose、关键变量/控制流、简洁但不遗漏行为，表明 LLM 可以作为 learn-by-example 资源。

## Ablation / Error Analysis
学生解释之间质量差异较大，学生自己写解释仍有学习价值，不能因 LLM 文本评分更高就删除 active explanation practice。样本函数、评审熟悉度和 LLM prompt 会影响结果；“感到易懂”不等于每个技术细节都正确。

## Limitations
这是感知/问卷比较，不是长期学习增益或真实 code-writing improvement；LLM 版本、函数数量和学生群体有限。评审者可能偏好流畅语言，且生成解释仍需教师核验。

## 两句话总结
在大班 CS 教育中，LLM 生成的代码解释平均比学生解释更准确、更易懂，长度却不更长。它适合作为可扩展示例，但不应取代学生主动解释和对 hallucination/细节错误的人工审查。

## Evidence note
已读取 RQ、约 1000 人课程样本、三函数比较、评分/统计结果、学生偏好与 limitations。
