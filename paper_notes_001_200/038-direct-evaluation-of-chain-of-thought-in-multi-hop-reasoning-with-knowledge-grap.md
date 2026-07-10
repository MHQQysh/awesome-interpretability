# 038. Direct Evaluation of Chain-of-Thought in Multi-hop Reasoning with Knowledge Graphs

- **作者 / venue**：Jingye Chen, Jiaqing Liang, Siliang Xu, et al.；Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.168/)
- **任务**：直接评估 KG multi-hop CoT 是否事实正确、逻辑连贯并导向正确答案

## 1. Introduction

CoT 的最终答案正确不代表中间步骤正确：模型可能凭记忆猜中答案，也可能在错误 reasoning chain 后得到正确结论。自由文本 CoT 还难以自动验证，尤其是知识图谱问题中每一步应对应实体和关系。

论文因此定义可检查的 reasoning criteria：无 factual errors、步骤之间 coherent、最后一步能够支持正确 answer，并将 CoT 转成结构化格式用于细粒度 evaluation。

## 2. Method

- 使用 KG 事实检查每个 reasoning step 是否存在。
- 检查当前 step 是否由上一步 prerequisite 支持，防止跳步或关系断裂。
- 检查整个路径是否通向最终答案。
- 对 LLM 生成的结构化 CoT 做自动 evaluation，并用案例分析区分知识不足、关系错误和推理顺序错误。

## 3. Baseline 与对比

- final-answer accuracy：传统只看下游答案的 baseline。
- few-shot CoT：有推理文本但没有逐步 KG verification。
- self-consistency / better prompting：提高答案稳定性但不保证每一步事实正确。
- 多个 LLM（包括较大闭源/开源模型），比较 answer accuracy 与 reasoning accuracy 的差距。

## 4. Findings

- LLM 的 final answer accuracy 通常高于 faithful reasoning accuracy；模型可能“知道答案”但不能生成正确的中间路径。
- Vicuna 等模型在答案上可以有合理表现，但 reasoning validity 较低，说明 answer score 会高估推理能力。
- 大模型更容易生成有效步骤，但在知识缺失或多跳关系复杂时仍会出现 factual error、coherence error 和 answer-link error。
- 结构化 CoT 让错误可定位，为训练或评测 faithful reasoning 提供了比自由文本 rationale 更直接的接口。

## 5. 局限

KG 覆盖不足会把合理推理误判为错误；自动规则难以覆盖开放世界常识；结构化输出格式也可能改变模型原始 CoT 行为。

**一句话评价**：论文把 CoT 的评价单位从“整条解释”拆到知识图谱可验证的 reasoning step，证明答案正确不能代表推理链忠实。
