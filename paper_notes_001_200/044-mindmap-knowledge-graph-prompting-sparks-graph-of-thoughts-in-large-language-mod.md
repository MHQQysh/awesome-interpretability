# 044. MindMap: Knowledge Graph Prompting Sparks Graph of Thoughts in Large Language Models

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.558/)
- **任务**：用知识图谱提示组织 LLM 的多跳推理和外部证据

## Introduction

LLM 的推理具有流畅性，但会产生 hallucination、知识过时和参数更新困难。普通 document retrieval 只把相关文本拼到 prompt，难以表达实体-关系结构；KG-only 方法又可能过于僵硬，不能利用 LLM 的隐式知识。

## Method

MindMap 将 KG evidence 与 multi-chain-of-thought 结合：模型理解实体和关系，再沿图结构展开多个 reasoning branches，最后合并为 answer。它试图让 LLM 同时使用参数化知识和外部结构化知识，并通过图路径减少无根据的自由联想。

## Baseline 与对比

- LLM-only prompting。
- document retrieval + LLM。
- KG retrieval + LLM。
- 不同 graph prompting 组件和 hallucination benchmark。

## Findings

- KG prompting 能改善实体关系推理和高风险问题的证据可追溯性。
- graph-of-thoughts 比单链 CoT 更适合多个候选路径和相互验证。
- 外部 KG 可以缓解 outdated knowledge，但 KG 的覆盖、错误和路径选择仍会影响最终答案。
- 论文还分析不同组件，说明 MindMap 的收益来自 KG comprehension、路径组织和多链推理的组合，而不是只增加检索文本。

## 局限

图构建和实体链接成本高，开放世界中不存在的边不能简单判定为错误；图路径也可能给模型虚假的确定感。

**一句话评价**：MindMap 把外部知识从“文本片段”升级成“可展开的推理图”，为解释多跳推理提供了结构化中间对象。
