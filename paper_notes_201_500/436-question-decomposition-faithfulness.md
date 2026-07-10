# 436. Question Decomposition Improves the Faithfulness of Model-Generated Reasoning

- **Authors:** Ansh Radhakrishnan, Karina Nguyen, Anna Chen, Carol Chen, Carson Denison, Danny Hernandez, Esin Durmus, Evan Hubinger, and others
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2307.11768
- **Tags:** `faithfulness` `chain-of-thought` `question-decomposition` `reasoning`

## Introduction
CoT 的吸引力在于把答案过程外显出来，但外显 reasoning 可能只是事后辩护：模型会忽略其中的错误、或把没有说出的偏见藏起来。本文不把“更长的解释”当成更可信，而是研究能否通过改变任务分解结构，让每一步更接近模型真正用于回答的局部计算。

## Method / Framework
论文比较普通 CoT、chain-of-thought decomposition 和 factored decomposition。CoT 一次生成步骤和最终答案；CoT decomposition 先生成一串更简单的 subquestions，并在同一上下文回答；factored decomposition 也生成 subquestions，但每个 subquestion 在新的独立 context 中回答，再把子答案带回原问题重组。独立 context 减少模型利用原问题中的 spurious demographic cues 或未声明偏见，也减少“跳过了某个隐含推理步骤”的情况。

作者用四类 faithfulness 测试：截断 reasoning 后最终答案是否变化、破坏 reasoning 后答案是否变化、加入 biased context 后准确率变化，以及任务本身的 QA accuracy。核心思想是：如果模型真的依赖 stated reasoning，截断或篡改其中的关键步骤应显著改变答案；如果答案对这些改动不敏感，解释就可能是 ignored reasoning。

## Baselines / Comparisons
基线包括 zero-shot、few-shot、standard CoT、CoT decomposition 和 factored decomposition。所有主要方法使用高质量 few-shot demonstrations，并在四个 QA 任务上平均报告 accuracy 与三项 faithfulness 指标。比较的不是单个分数，而是 performance-faithfulness Pareto frontier，因为更高准确率若依赖更不忠实的解释并不能解决安全审计问题。

## Experiments / Findings
表 1 的平均结果为：zero-shot accuracy 72.8，few-shot 79.7，CoT 86.0，CoT decomposition 85.6，factored decomposition 81.8。CoT 的 accuracy 最高，但 factored decomposition 的 faithfulness 最好：final-answer truncation sensitivity 从 CoT 的 10.8/CoT decomposition 的 11.7 提升到 20.5，final-answer corruption sensitivity 从 9.6/28.7 提升到 33.6；biased-context accuracy change 则从 CoT 的 -11.3 降到 factored decomposition 的 -3.6。

结果说明分解方法牺牲部分 CoT 的峰值准确率，却显著改善“答案是否真正依赖展示出来的中间步骤”。CoT decomposition 是折中方案：保留单 context 带来的部分性能，同时比普通 CoT 更能暴露被模型使用的子问题结构；factored decomposition 则在 faithfulness 上更接近安全验证所需的行为。

## Ablation / Error Analysis
主要对照是 context 是否独立，而不只是是否把问题写成 subquestion。factored decomposition 的优势正来自切断原问题背景对每个子答案的潜在污染；如果所有子问题仍在一个 context 中生成，模型可能通过隐式信息捷径得到正确答案。failure cases 包括分解本身不合理、子问题答案错误、重组时忽略子答案，以及 faithfulness proxy 只能测到特定类型的 ignored/bias reasoning。

需要谨慎解释截断/破坏敏感度：低敏感度可能表示模型没用 reasoning，也可能是答案有冗余路径；高敏感度也不自动证明 reasoning 是语义上正确的。论文的 Pareto 图因此强调“更可检验”而不是“已经完全 faithful”。

## Limitations
分解会增加多轮调用和 token 成本，并且依赖模型自己提出合适的子问题；小问题或需要整体上下文的问题不一定适合 factored context。faithfulness 指标是行为 proxy，不能直接观察模型内部过程；实验任务和高质量示例有限，不能外推到所有开放式推理、安全决策或大规模 agent 工作流。

## 两句话总结
本文通过让 LLM 把复杂问题拆成独立、可验证的子问题，降低 CoT 中被忽略推理和未声明偏见，使 stated reasoning 更能预测最终答案。factored decomposition 的平均准确率低于 CoT，但截断、破坏和 biased-context 测试都更 faithful，展示了性能与可审计性之间的可量化 Pareto 权衡。

## Evidence note
已逐段读取本地 PDF，核对了三种 reasoning prompting、四项评价指标，以及表 1 的 accuracy、truncation/corruption sensitivity 和 biased-context 数值。
