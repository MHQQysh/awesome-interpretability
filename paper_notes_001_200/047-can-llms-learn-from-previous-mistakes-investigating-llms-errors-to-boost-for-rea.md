# 047. Can LLMs Learn from Previous Mistakes? Investigating LLMs’ Errors to Boost for Reasoning

- **作者 / venue**：ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.169/)
- **任务**：从错误 CoT 中提取错误类型，进行 self-rethinking 与 mistake tuning

## 1. Introduction

已有 CoT 工作通常给模型 golden rationale，但人类学习不仅来自正确例子，也来自错误和纠错。论文问：LLM 能否识别自己的 reasoning error、抽象错误类型，并利用这些错误改善后续推理？

## 2. Method

- **Self-rethinking**：模型先生成 answer/CoT，再检查其中的 calculation、numeric、logical、common-sense、linguistic、context error。
- 若发现错误，模型重新生成 rationale 并循环评估，直到通过或达到迭代上限。
- **Mistake tuning**：把错误及其修正过程整理为训练/提示信号，让模型学习常见错误模式。
- 对 arithmetic、logical reasoning、commonsense 等任务做分领域错误分析。

## 3. Baseline 与对比

- standard CoT / few-shot CoT。
- self-consistency 和 self-reflection 类方法。
- 只给正确 rationale 的 reasoning baseline。
- 多个 reasoning benchmark，包括 LogiQA 等，比较错误类型和修正后的 accuracy。

## 4. Findings

- 模型可以从自身 CoT 发现部分错误，并通过 self-rethinking 改善答案。
- 将错误归为抽象类别比逐字重复错误更有用：不同题目中的 numeric/logical/context error 可以共享修正策略。
- mistake tuning 提供了利用错误数据的低成本路线，不必为每个任务重新收集大量 golden rationale。
- 但模型有时无法识别自己的错误，尤其在初始答案很自信或错误来自知识缺失时；自我检查可能再次复制原错误。

## 5. 局限

错误分类器和 self-check prompt 会影响结果；多轮重思考增加成本；模型生成的“错误解释”不一定是真实错误机制。需要外部 verifier、人工标注和跨任务测试。

**一句话评价**：论文把错误本身作为 reasoning supervision，展示 LLM 可以从 mistake pattern 中获益，但 self-rethinking 仍不是可靠的独立验证器。
