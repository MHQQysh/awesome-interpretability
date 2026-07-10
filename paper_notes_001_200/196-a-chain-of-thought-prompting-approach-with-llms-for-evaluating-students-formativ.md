# 196. A Chain-of-Thought Prompting Approach with LLMs for Evaluating Students' Formative Assessment Responses in Science

- **Authors:** Clayton Cohn, Nicole Hutchins, Tuan Anh Le, Gautam Biswas
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/30364
- **Tags:** CoT Evaluation, Education, Rubric, Human-in-the-Loop

## Introduction

短答案科学作业不仅要判分，还要说明学生错在概念还是推理；传统自动评分常能给分却不能给出与 rubric 对齐的证据。论文在中学 Earth Science formative assessment 中研究 GPT-4 是否能通过 chain-of-thought 和少量人工反馈稳定地评分并解释，而不是把模型输出直接当成教师结论。

## Method / Framework

流程以 rubric、题目和学生答案为输入，让 GPT-4 先生成 CoT，再输出分数及与 rubric 绑定的证据/解释。作者逐步加入 active learning：从 zero-shot（只有 rubric）、few-shot（加入带分数样例）、few-shot CoT（样例含 reasoning）到 CoT + Active Learning，根据模型困难或不确定样本补充标注例子，形成 human-in-the-loop prompt engineering。

## Baselines / Comparisons

四个设置本身构成增量 baseline：Zero-Shot、Few-Shot、Few-Shot+CoT、CoT+AL。评估使用 held-out test，报告 accuracy、Macro-F1、Cohen's quadratic weighted kappa (QWK)，并按 Arrow Size、Arrow Direction、Reasoning 等概念/推理子项分析；此外人工研究团队审阅模型与人类不一致案例。

## Experiments / Findings

- 对简单概念项，few-shot 或 CoT+AL 接近完美：Q1 Arrow Size 的最佳设置约达到 0.98 accuracy/F1，说明 rubric 和少量示例足以解决定义清楚的子任务。
- 对 Q2 的总分推理，zero-shot/few-shot 较弱，而 few-shot CoT 与 CoT+AL 明显提高；CoT+AL 达到 0.85 accuracy、0.79 F1、0.87 QWK，说明推理样例和针对性反馈主要帮助歧义较大的问题。
- 模型生成的证据通常能连接到 rubric，具有教师可读性；22 个模型-人工分歧中人工复核仅有 3 个案例与模型一致，显示多数错误仍需要领域专家审查，而不是盲信自动解释。

## Ablation / Error Analysis

逐步 baseline 说明 CoT 和 active learning 的增益并不均匀：简单题可能过拟合 prompt，复杂题则受益于 reasoning demonstrations。错误包括关键词触发、把过细 CoT 当作概念证据、学生答案的科学表述歧义，以及人类评分者本身缺乏共识；因此模型与人类的 disagreement 也反映 rubric 质量，而不只是模型能力。

## Limitations

数据来自有限的中学 Earth Science 课程，不能代表其他年级、学科或开放式写作。学生数据隐私、偏见、幻觉和高风险课堂决策仍需保护；CoT 是否真实驱动最终分数也未被因果验证，且 active learning 需要教师持续参与。

## 两句话总结

论文把 GPT-4 的 CoT、rubric 证据和 active learning 结合，用于给科学短答案评分并生成教师可读解释。它在简单概念题上接近人类、在复杂推理题上明显受益于 CoT，但模型解释仍需教师复核，不能直接替代教育评估责任。
