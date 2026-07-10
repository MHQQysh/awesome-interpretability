# 152. The CoT Collection: Improving Zero-shot and Few-shot Learning of Language Models via Chain-of-Thought Fine-Tuning

- **Authors:** Seungone Kim, Se June Joo, Doyoung Kim, Joel Jang, Seonghyeon Ye, Jamin Shin, Minjoon Seo
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.782
- **Tags:** CoT Fine-tuning, Instruction Tuning, Rationale, Generalization

## Introduction

100B 以上的 LLM 才普遍能从 few-shot CoT 受益，小模型即使理解 instruction，也常无法产生可靠的逐步推理。已有 instruction tuning 主要包含少量 CoT tasks，与总 instruction task 数量不匹配，导致小模型在 unseen reasoning task 上泛化弱。

## Method / Framework

作者构造 CoT Collection：在原 Flan Collection 基础上，把 CoT rationale 扩展到 1,060 个 task，共 1.84M 条 rationale。用 GPT-3.5 / GPT-4 等 teacher 生成或整理 reasoning chain，并过滤不一致、无效和过长样本，再微调 Flan-T5 3B / 11B，得到 CoT-T5。

评测分 zero-shot unseen tasks、few-shot task-specific 和多语言 MGSM；除了 final answer accuracy / EM，还人工检查 rationale 的 faithfulness、semantic alignment、informativeness、logical inference 和 coherence。

## Baselines / Comparisons

比较原始 Flan-T5、T0、Tk-Instruct、M-T5、CoT prompting、直接 fine-tuning、ChatGPT / Codex / Vicuna，以及只含 9 个 CoT task 的 instruction tuning。任务包括 BIG-Bench-Hard 23/27 个任务、P3、4 个领域数据集和 5 种语言的 MGSM。

## Experiments / Findings

- BBH zero-shot 平均提升：CoT-T5-3B +4.34%，CoT-T5-11B +2.60%。
- 在 4 个 domain-specific tasks 上，3B / 11B 分别提升约 2.24% / 2.37%；CoT-T5 甚至比提供最大长度 demonstrations 的 ChatGPT 高约 13.98%。
- CoT Collection 比只用少数 CoT task 的 Flan 数据更能改善未见任务，说明 task diversity 比单纯增加同一任务 rationale 更重要。
- 多语言 MGSM 也有收益，证明 CoT fine-tuning 能把推理格式迁移到非英语，但语言间收益不完全一致。
- rationale 人工质量较高：semantic faithfulness、informativeness 和 coherence 较好，但 logical discourse representation 明显更难，说明“写得像推理”不等于逻辑完整。

## Ablation / Error Analysis

作者消融 CoT task 数、rationale 过滤、3B / 11B 规模、direct answer 与 CoT target、不同语言和 in-domain retention。只增加更多 noisy rationale 会伤害效果；数据中任务分布不均时会导致某些 reasoning type 被过度学习。CoT fine-tuning 可能造成普通 instruction task 的轻微遗忘，因此需要平衡 CoT 与 direct-answer 数据。

## Limitations

大规模 rationale 依赖 teacher LLM，可能继承 hallucination、错误步骤和风格偏差；人工质量评估不能覆盖 1.84M 样本。训练成本、模型 checkpoint 与 prompt 细节影响复现；CoT 文本的可读性不能证明模型的内部推理忠实。更小模型和更复杂数学任务仍可能接近零准确率。

## 两句话总结

CoT Collection 通过把 rationale 扩展到 1,060 个任务，让 3B / 11B Flan-T5 获得更强的 unseen-task CoT 能力。收益来自多任务推理覆盖和格式迁移，但 rationale 的逻辑正确性、teacher 噪声与潜在遗忘仍需要严格控制。
