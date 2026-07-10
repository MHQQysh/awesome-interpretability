# 058. Med-HALT: Medical Domain Hallucination Test for Large Language Models

- **作者 / venue**：CoNLL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.conll-1.21/)

## Introduction 与方法

医疗 hallucination 可能影响诊断、治疗和生命安全，普通开放域 benchmark 不能反映医学术语、事实和推理风险。Med-HALT 提供 medical-domain hallucination test，区分 reasoning hallucination 与 knowledge/factual hallucination，并分析模型在不同医疗问题上的弱点。

## Baseline 与评价

使用 OpenAI Text-Davinci 等 LLM，比较不同 prompting/任务类型；指标包括正确预测比例和 hallucination 类型统计。重点不是只给一个总分，而是看模型在哪种医学推理中更容易自信出错。

## Findings

模型在医学知识缺失、复杂诊断链和需要多步推理的问题上更容易生成 plausible but unverified information。医疗场景中语言流畅会增加风险，因为用户可能把不确定回答当成专业建议。

## 局限与安全

医疗 benchmark 不能替代临床试验或专业人员；知识更新、地域指南和病人具体情况会改变答案。应用中应配合检索、拒答、专业审核和风险分级。

**一句话评价**：Med-HALT 把 hallucination evaluation 放到医疗高风险域，强调“正确率下降”之外还要识别错误类型和潜在伤害。
