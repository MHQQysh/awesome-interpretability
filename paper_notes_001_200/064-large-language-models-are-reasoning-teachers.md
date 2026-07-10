# 064. Large Language Models Are Reasoning Teachers

- **作者 / venue**：ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-long.830/)

## Introduction / Method

超大模型可通过 zero-shot-CoT 生成 reasoning，但直接部署成本高。Fine-tune-CoT 让 GPT-3 175B 等 teacher 生成 reasoning samples，再用这些 rationale 训练小 student，研究 reasoning 能力是否可以蒸馏。

## Baseline

比较 zero-shot、zero-shot-CoT、manual/few-shot CoT、直接 fine-tuning 和 teacher/student 规模。还比较多条 teacher rationale 与单条 rationale 的数据增强效果。

## Findings

teacher-generated CoT 能显著提升小模型在复杂推理任务上的表现，甚至在较小 student 上也可见 reasoning behavior。多样 rationale 增加 supervision 覆盖，但蒸馏出的能力不等于 student 复现了 teacher 的内部机制。

## 局限

teacher rationale 可能包含错误、冗余或不可忠实步骤；student 可能学习答案模式而非推理。需要过程级评价和外部 verifier。

**一句话评价**：论文证明大模型可以作为 reasoning teacher，但 CoT distillation 的性能提升不能直接证明推理解释 faithful。
