# 122. Verification and Refinement of Natural Language Explanations through LLM-Symbolic Theorem Proving

> 人工精读笔记：EMNLP 2024。重点是用 theorem prover 验证 explanation 的逻辑有效性，再把反馈返回给 LLM 修订。

## 0. 论文信息

- **作者**：Xin Quan, Marco Valentino 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.172)
- **代码**：[Explanation-Refiner](https://github.com/neuro-symbolic-ai/explanation_refinement)
- **数据**：e-SNLI、QASC、WorldTree；Isabelle/HOL

## 1. Introduction：要解决什么问题

自然语言 rationale 可读但可能包含逻辑跳步、矛盾或不支持结论；仅用 NLI/LLM judge 评价 explanation 需要大量人工标注，且容易把流畅性当作逻辑有效性。作者将 explanation formalize 成事实集合和 entailment theorem，让符号 prover 给出硬约束。

## 2. Method / Framework：怎么解决

Explanation-Refiner 由 LLM 生成解释、自动形式化 facts/relations、theorem prover 构造 deductive proof 三部分组成。若 prover 不能证明 premise entails hypothesis，系统用外部反馈定位无效 fact，再让 LLM 删除、改写或补充 explanation，循环至通过或达到上限。

## 3. Baseline / 对比基线

原始 LLM explanation、仅 LLM 自我反思、NLI/entailment scoring、人工解释、不同 LLM backbone（GPT-4、Mistral、Llama）。另比较有无 theorem-prover feedback 的 refinement。

## 4. Experiments / Findings：结果如何读

Explanation-Refiner 在 e-SNLI、QASC、WorldTree 上显著提高逻辑有效性；摘要报告 GPT-4 explanation 通过率从约 36% 提升到 84%、12% 到 55%、2% 到 37%，具体取决于数据集。Mistral/Llama 也有改善，说明外部 prover 反馈能纠正一部分 LLM autoformalization 和推理错误。

系统不只验证最终标签，还验证中间 facts 的可证明性；因此能发现“答案对但理由错”的情况，这是单纯 accuracy 无法暴露的。

## 5. Ablation / 机制解释

去掉 theorem prover、只让 LLM self-refine、不同 explanation complexity、不同 backbone 和 proof feedback granularities 构成主要消融。LLM-symbolic loop 的收益来自硬逻辑约束，而非额外生成轮次本身。

## 6. Limitations / 局限与复现注意

自然语言到形式逻辑的自动翻译可能错误；theorem prover 覆盖的逻辑语言有限；多轮 formalization/refinement 成本高；通过 prover 不等于 explanation 与真实神经内部机制一致。

## 7. 两句话总结

论文把自然语言 explanation 转成可由 theorem prover 检查的逻辑事实，并将 proof failure 反馈给 LLM 修订。它在三个 NLI/科学推理数据集上大幅提高 explanation validity，但形式化覆盖、自动翻译误差和推理成本限制了开放域应用。
