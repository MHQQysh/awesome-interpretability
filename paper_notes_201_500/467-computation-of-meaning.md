# 467. On the Computation of Meaning, Language Models and Incomprehensible Horrors

- **Author:** Michael Timothy Bennett
- **Venue / year:** AGI 2023 / arXiv 2023
- **Paper:** [arXiv:2304.12686](https://arxiv.org/abs/2304.12686)
- **Tags:** `meaning` `AGI` `grounding` `interpretability` `philosophy-of-LLM`

## Introduction
文章讨论一个比“模型是否会生成正确句子”更基础的问题：LLM 的符号是否具有说话者意图、听者识别和环境耦合意义。作者认为纯文本统计能复制语法行为，却不足以证明模型理解人类所说的 meaning。

## Method / Framework
这是理论综合，不是训练模型实验。作者以 Grice 的 m-intention、truth-conditional semantics、Peircean semiotics 和 enactive cognition 的可计算 formalism 为桥梁，把 task、goal、agent/environment、implementable language、symbol system 与 communication 条件形式化，再检查语言模型是否满足这些条件。

## Baselines / Comparisons
不存在神经模型 benchmark baseline；核心对照是 top-down 哲学/语言学意义理论与 bottom-up LLM syntax modeling。论文还讨论具备 theory-of-mind-like behavior 不等于有意图或真正 communication，并提出与人类 feelings/grounded interaction 相关的未来构造方向。

## Experiments / Findings
主要结论是当前 ChatGPT 类模型可以产生高度人样的 utterance，却没有被证明拥有与人类相同的 comprehension 或 m-intention；因为训练目标优化的是文本延续，不是任务中的传感运动反馈、目标和听者识别。作者提出通过模拟 human feelings、构造 weak representations 和优化 grounded tasks，才可能逐步满足 meaningful communication 的条件。

## Ablation / Error Analysis
没有数值 ablation 或模型控制实验；“解释”来自理论条件逐项检查。文章的反例逻辑是：复杂 theory-of-mind 行为、语义连贯和正确回答都可能由 syntax/统计相关性产生，因此不能单独作为 meaning 的证据。

## Limitations
结论依赖对 meaning、intent 和 consciousness 的理论选择，难以用单一行为 benchmark 判定。它不提供可直接复现的 LLM intervention，也没有区分不同模型、工具调用或多模态 grounding 的实证程度。

## 两句话总结
论文把 LLM 可解释性问题提升为“模型是否有可计算的意义与意图”，并用 Grice/符号学/具身认知 formalism 说明文本统计不等于理解。它是理论边界论文，不是新解释算法，因此最有价值的是明确了行为流畅、语义解释和 grounded meaning 之间的证据鸿沟。

## Evidence note
已读取 arXiv 正文的 meaning formalization、communication、talking-to-a-machine 和 conclusion；未将理论主张写成实验事实。
