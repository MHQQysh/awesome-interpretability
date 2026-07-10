# 480. Capabilities of GPT-4 on Medical Challenge Problems

- **Authors:** Harsha Nori, Nicholas King, Scott Mayer McKinney, Dean Carignan, Eric Horvitz
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2303.13375](https://arxiv.org/abs/2303.13375)
- **Tags:** `medical-llm` `calibration` `self-explanation` `high-stakes-evaluation`

## Introduction
论文建立 GPT-4 在医学挑战题上的基线，重点不止是 accuracy，还包括图像缺失时的推理、GPT-4-base 与 RLHF release 的差异、概率 calibration，以及模型能否给学生解释和构造 counterfactual medical cases。

## Method / Framework
评估两套 USMLE official practice materials（Self Assessment/Sample Exam）和 MultiMedQA 的 MedQA、PubMedQA、MedMCQA、MMLU medical subsets；比较 GPT-4 zero/5-shot、GPT-3.5、ChatGPT、Flan-PaLM/Med-PaLM 文献结果。对每道选择题读取选项概率画 calibration curve，并用 case study 让 GPT-4解释答案、按学生程度改写、生成反事实场景。

## Baselines / Comparisons
USMLE 与 GPT-3.5/ChatGPT 对照，MultiMedQA 与 GPT-3.5、Flan-PaLM 540B、GPT-4-base 对照；GPT-4-base 用来隔离 alignment/safety tuning。还分 text-only 与题目提到但未传给模型的 image/graph questions。

## Experiments / Findings
USMLE Self Assessment GPT-4 overall 5-shot/zero-shot 为 86.65/83.76，GPT-3.5 为 53.61/49.10；Sample Exam GPT-4 为 86.70/84.31，GPT-3.5 为 58.78/56.91。GPT-4 在未看到媒体的题上仍达 Self Assessment 69.75/68.15、Sample Exam 79.59/75.51，说明文本临床线索与 test-taking logic 能部分替代图像，但不能证明视觉理解。

MultiMedQA 上 GPT-4 超过 GPT-3.5 和 Flan-PaLM 的多数数据集；GPT-4 calibration 明显更好，例如模型给 0.96 平均置信度的样本约 93% 正确，而 GPT-3.5 相同置信度约 55%。GPT-4 case study 能解释临床理由、按学生调整深度并生成 counterfactual，但这些是生成能力展示，不是医学事实完备证明。

## Ablation / Error Analysis
GPT-4-base 在 14 个数据集总体比 aligned release 高约 3--5%，显示 safety/instruction tuning 可能牺牲 raw medical accuracy。5-shot 与 zero-shot、text/media、英文/中文/繁体中文和不同 benchmark 的差异揭示了 prompt、视觉缺失、语言和数据泄漏风险。

## Limitations
模型参数/训练数据未知，USMLE practice materials 可能被预训练记忆；多选 calibration 不能代表长文本诊断/治疗建议。研究不提供临床 prospective trial，生成的解释与 counterfactual 仍需专家核验，不能用考试成绩替代安全性证据。

## 两句话总结
GPT-4 在 USMLE 和 MultiMedQA 上显著超过 GPT-3.5，并表现出更好的多选 calibration 和可交互医学解释能力。论文同时显示 release alignment 可能带来 3--5% accuracy trade-off，且文本线索、记忆、幻觉和临床风险使 benchmark 不能等同于真实医疗可靠性。

## Evidence note
已读取 USMLE/MultiMedQA 主表、media question 分析、GPT-4-base 对照、calibration、case study 和 limitations。
