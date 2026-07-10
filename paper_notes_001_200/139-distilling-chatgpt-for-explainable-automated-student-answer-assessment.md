# 139. Distilling ChatGPT for Explainable Automated Student Answer Assessment

- **Authors:** Jiazheng Li, Lin Gui, Yuxiang Zhou, David West, Cesare Aloisi, Yulan He
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.399
- **Tags:** Rationale Generation, Education, Distillation, Evaluation

## Introduction

自动学生答案评估若只输出分数，学生不知道缺少哪些关键点，教师也难以解释分类器的决策；而人工反馈昂贵且不同教师之间标准不一致。已有 BERT / Longformer 评估器能提高效率，却通常是黑盒分类，数据集又只有 score 标注，没有可直接用于 rationale generation 的解释标注。

论文提出 AERA（Automated Explainable Student Response Assessment）：先用 ChatGPT 作为 reasoning teacher 生成分数和 rationale，再通过数据与 rationale refinement 清理噪声，最后蒸馏到可本地运行的 Long T5，使小模型同时输出评分与解释。

## Method / Framework

1. **Prompt selection。** 比较 Simple Instruction、Complex Instruction 和 Example Instruction。输入包括 question、key elements、rubric、student answer，输出 score 与理由；few-shot example 还规定格式并减少自由生成。
2. **Data refinement。** 用多次 ChatGPT 输出的语义置信区间找出疑似错误标签；修正明显错误的原始分数，并用给定 score 作为先验，让 XY-to-R prompt 重新生成更聚焦的 rationale。
3. **Student model。** 用 Question、Key Elements、Rubric、Student Answer 和 Score/Rationale 微调 Long T5，得到 AERA。
4. **Evaluation。** 在 Hewlett Foundation ASAP-SAS 的 4 个 Science / Biology 子集上测 Acc、macro-F1、QWK；rationale 用 sacreBLEU 选择 checkpoint，再用人工标注评估关键元素是否正确、rubric 是否被忠实使用以及 AERA / ChatGPT 哪个解释更好。

## Baselines / Comparisons

分类 baseline 是 BERT、Longformer、Longformer-all（额外加入 question、key elements、rubric）。LLM prompting baseline 是 Simple、Complex、Example Instruction 的 ChatGPT。可解释生成 baseline 是 AERA、去掉错误标签修正、去掉 rationale refinement，以及 Correct Score Only。比较同时覆盖黑盒分类性能与解释质量，避免只用 QWK 评价可解释系统。

## Experiments / Findings

- Overall QWK：BERT 75.22，Longformer 81.61，Longformer-all 81.14；ChatGPT Simple / Complex / Example 分别为 49.68 / 57.03 / 64.36；AERA 为 71.62。AERA 没达到最强分类器的分数，但显著优于直接 ChatGPT prompting，并同时提供 rationale。
- 在数据集 #5 和 #6 上，AERA 的 QWK 76.44 / 80.81，超过 BERT；说明生成式评分在某些 subject 上能受益于更长输入和 rubric reasoning。
- Example Instruction 比 zero-shot prompt 稳定且更强，说明少量结构化 assessment examples 能帮助 ChatGPT 对齐输出格式和评分标准。
- AERA 通过 refinement 后的 Long T5 生成的 rationale 在人工评测中总体优于 ChatGPT，评审认为它更少泛泛而谈、更能指向 key elements 和 rubric。
- 论文用高置信 ChatGPT 预测发现部分原始 ASAP-SAS 标注疑似错误；这既提高了训练数据质量，也说明“蒸馏 teacher”不能盲信 gold label。

## Ablation / Error Analysis

Correct Score Only 在 #1、#5、#6 有时优于加入所有 refinement 的数据，但在 #2 数据不足时变差；Fixing Wrong Labels 与 Rationale Refinement 结合后能增加有效训练数据。去掉任一组件，overall QWK 从 71.62 降到约 57.78 / 57.75；只保留 score 不足以学习忠实 rationale。错误分析还显示，学生答案含糊或 key elements 中有 “other acceptable responses” 时，ChatGPT 很难判断哪些内容真正满足 rubric。

## Limitations

AERA 的训练标签和解释来自 ChatGPT，teacher 的 hallucination 或错误评分可能被蒸馏进 Long T5；依赖“预测分数正确则 rationale 支持该分数”的假设并不总成立。数据只有 4 个学科子集，QWK 主要评价分数而不是解释的因果忠实性。人工评测规模有限，Long T5 生成的理由仍可能是事后合理化。

## 两句话总结

AERA 把 ChatGPT 的评分与解释能力蒸馏到 Long T5，并用置信度、错误标签修正和 rationale refinement 处理缺少解释标注的问题。它在保留可解释输出的同时显著超过直接 ChatGPT prompting，但 teacher 噪声、数据集标注质量和 explanation faithfulness 仍是关键风险。
