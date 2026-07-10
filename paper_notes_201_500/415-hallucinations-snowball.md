# 415. How Language Model Hallucinations Can Snowball

- **Authors:** Muru Zhang, Ofir Press, William Merrill, Alisa Liu, Noah A. Smith
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2305.13534
- **Tags:** `hallucination` `self-conditioning` `reasoning` `error-propagation`

## Introduction
多轮生成和 chain-of-thought 中，模型会把自己前面生成的内容当作上下文。如果早期句子有一个小错误，后续步骤可能不再重新核验，而是围绕错误继续推理，形成 snowballing hallucination。本文量化这一错误放大过程，并比较不同 prompting/模型条件。

## Method / Framework
实验让 LLM 逐步回答需要多步推理的问题，区分 first-step error 和后续 dependent error。作者通过中途插入错误、比较逐步生成与直接回答、以及观察模型是否修正 earlier statement，测量错误如何累积。

## Baselines / Comparisons
比较 direct answer、CoT/step-by-step generation、不同模型规模、不同 task difficulty 和有无外部 feedback/verification。指标包括 final accuracy、step accuracy、错误后恢复率和错误传播概率。

## Experiments / Findings
一旦 early step 出错，后续生成经常把错误当成前提，导致最终答案比独立采样更差；错误不是简单相加，而会随着依赖链 snowball。CoT 提高了某些任务的平均正确率，却也提供了更多机会把错误中间状态写入上下文。

## Ablation / Error Analysis
错误注入位置、链长、任务难度和模型规模是主要消融。短链或容易任务的错误更可能被纠正，长链、多步 arithmetic/knowledge reasoning 更容易持续错误。self-consistency/re-sampling 若共享相同错误先验，也不能保证恢复；外部验证和重新 grounding 是更直接的控制。

## Limitations
错误注入与自然 hallucination 不完全相同，任务多为可判定 benchmark，不能覆盖开放对话。模型版本、prompt 和 sampling 对传播率敏感。论文分析行为链，不直接定位哪一层/哪一神经元造成错误累积。

## 两句话总结
本文显示 LLM 会把早期错误写进后续上下文并不断自条件化，使多步推理中的 hallucination snowball 而不是被自动纠正。CoT 的收益和风险并存，实际系统需要逐步验证、外部证据或允许回滚，而不能只相信更长的 rationale。

## Evidence note
已读取 arXiv 2305.13534 本地 PDF 的错误传播设定、逐步生成对比、错误注入和结论。
