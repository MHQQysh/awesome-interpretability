# 052. Characterizing Large Language Models as Rationalizers of Knowledge-intensive Tasks

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.484/)

## Introduction 与方法

知识密集任务的 rationale 需要支持答案、反驳其他选项，而不只是生成流畅理由。论文在 commonsense multiple-choice QA 等任务中，让 LLM 基于题目、选项和外部 ConceptNet/QAGNN 风格知识生成 rationale，并研究 rationale 的可信性、可读性和与真实决策的关系。

## Baseline 与评价

以 model-generated rationale、知识抽取策略和 ECQA 人类/数据集 rationale 做对比；使用 head-to-head 人评及 7-point Likert 评价 agreement、confidence、reliability、convincingness、conciseness 等。

## Findings

LLM rationale 能帮助用户理解答案，但 convincingness 可能高于客观 faithfulness。知识证据是否被正确使用、是否反驳错误选项，比 rationale 长度更重要。研究同时发现，模型可以生成听起来合理却没有真正支撑决策的解释。

## 局限

人评存在主观偏差，固定 250 个样本和 GPT-3.5 rationalizer 也限制泛化。后续需要 intervention、evidence deletion 和多模型验证。

**一句话评价**：论文把 rationale 的“会讲”与“可信”分开评价，说明知识密集任务中的自然语言解释不能只靠人类直觉判断。
