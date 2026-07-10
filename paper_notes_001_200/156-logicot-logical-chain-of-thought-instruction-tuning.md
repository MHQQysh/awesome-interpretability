# 156. LogiCoT: Logical Chain-of-Thought Instruction Tuning

- **Authors:** Hanmeng Liu, Zhiyang Teng, Leyang Cui, Chaoli Zhang, Qiji Zhou, Yue Zhang
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.191
- **Tags:** Logical Reasoning, Instruction Tuning, CoT, Self-Instruct

## Introduction

通用 instruction tuning 能提升对话和生成，却不一定教会模型复杂逻辑推理。GPT-4 的逻辑 CoT 质量很高，但闭源，社区需要能训练小模型的逻辑 reasoning instruction data。论文提出 LogiCoT，用 GPT-4 和已有逻辑推理样例生成带规则名、逐步证明和最终答案的训练集。

## Method / Framework

作者先收集逻辑任务的 premises、hypothesis、label 或 rule supervision，再设计 prompt，让 GPT-4 输出每一步使用的逻辑规则，例如 biconditional elimination、hypothetical syllogism、modus ponens。随后过滤格式错误、矛盾、无法从 premises 推出的 rationale，并以 instruction-input-CoT-output 形式微调开源模型。

LogiCoT 的目标不是只让模型输出 Yes / No，而是让它显式列出规则、事实和冲突点，减少自然语言 CoT 中跳步和循环论证。模型与数据权重都公开，便于比较 CoT prompting、普通 instruction tuning 和直接 fine-tuning。

## Baselines / Comparisons

比较 GPT-3.5 / GPT-4 prompting、Alpaca / Vicuna 类通用 instruction tuning、FLAN / T0、无 CoT 的 label-only tuning 和现有逻辑推理模型。任务涵盖 deductive reasoning、ruleTaker / AbductionAndNegation、FOLIO、ProofWriter 等逻辑与自然语言推理数据。

## Experiments / Findings

- LogiCoT fine-tuning 让较小模型在逻辑任务上获得更稳定的 step-by-step reasoning，通常超过只学最终标签的 baseline。
- 规则名称作为中间监督帮助模型区分蕴含、双条件、否定和反事实，解释更容易与 formal proof 对齐。
- 在未见逻辑任务上存在迁移收益，说明数据不只是背诵固定模板；但任务和语言形式变化大时，收益下降。
- GPT-4 生成的 rationale 在人工或自动规则检查中大多语义连贯，但仍有“结论正确、证明中间跳步”的样本，过滤和验证不可省略。

## Ablation / Error Analysis

消融 rule labels、rationale、self-instruction 数据量、过滤规则和不同 reasoning prompt。去掉规则名后模型仍能生成流畅 chain，却更容易使用错误 inference；只保留答案会提升格式简单任务，却损害复杂逻辑泛化。常见错误是把 converse 当 implication、忽略否定、从不充分条件推出结论和把 contradiction 当支持。

## Limitations

GPT-4 teacher 的错误可能被蒸馏，形式逻辑验证也不覆盖自然语言中的歧义。模型训练规模、任务模板和 proof annotation 仍影响结果；逻辑 CoT 的语言解释不一定是模型内部真正的搜索过程。复杂 FOL、量词、非单调推理和开放世界知识没有被完整覆盖。

## 两句话总结

LogiCoT 用 GPT-4 生成带逻辑规则的逐步证明，再对小模型做 instruction tuning，把“答案正确”推进到“能指出使用了什么规则”。它改善了逻辑推理和解释格式，但 teacher 噪声、规则覆盖与 rationale 忠实性仍需要自动证明器或人工验证。
