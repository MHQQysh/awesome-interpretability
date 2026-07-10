# 295. Faithfulness vs. Plausibility: On the (Un)Reliability of Explanations from Large Language Models

- **Authors:** Chirag Agarwal, Sree Harsha Tanneru, Himabindu Lakkaraju
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2402.04614
- **Tags:** Faithfulness, Plausibility, Self-Explanation, Explanation Evaluation

## Introduction

LLM self-explanations 读起来连贯、适应上下文，因此被广泛用于解释决策；但它们可能只是答案之后的合理化，不反映产生答案的真实过程。论文明确拆开 plausibility（人觉得像合理理由）与 faithfulness（理由是否忠实反映模型决策/依据），讨论为什么前者高不代表后者高。

## Method / Framework

工作以已有 self-explanation prompt 为对象，提出从 explanation quality、input evidence、model output consistency 和 perturbation/faithfulness test 观察二者分离。核心 evaluation 是对解释中的 token 做 masking/perturbation，看模型预测是否改变，并将 explanation 与模型行为、任务正确性和人类评价一起分析。

## Baselines / Comparisons

比较 prompting/self-explanation 与 attribution-based explanation、human rationale、random/irrelevant rationales 和不同 model/task conditions；也对照只评 plausibility 的人工/LLM judge 与需要模型行为改变的 faithfulness test。论文的贡献是提出评价警告和实验设计，而不是一个单一生成器。

## Experiments / Findings

文章显示 self-explanations 可以在语言上很可信，却与模型真实 decision process 不一致；解释与人类 rationale 的相似度、答案正确性和 perturbation faithfulness 可能不相关。模型能力、prompt、任务和是否正确分类都会改变 faithfulness，不能用一个“解释分数”泛化到所有 LLM。

## Ablation / Error Analysis

只看 plausible rationale 会奖励模板化、因果过度陈述和 post-hoc reasoning；只 mask explanation 也可能因分布外输入或 label bias 误判。需要 control rationale、answer-preserving/answer-changing perturbation、正确/错误样本分层和多个模型 judge。尤其不能把模型自己写出的 CoT 当作内部 trace。

## Limitations

faithfulness 仍用 behavioral proxy，不直接观察 logits/circuit；masking 粒度、prompt 和 human reference 会带来测量偏差。论文的主要证据来自任务级 self-explanation，不能覆盖所有 agent/多模态/工具调用场景。

## 两句话总结

这篇工作把“说得合理”和“真实忠实”明确分开，指出 LLM self-explanation 很容易高 plausibility、低 faithfulness。它要求解释评价与模型行为、反事实扰动、正确性和人工 rationale 同时结合，避免被流畅的事后理由欺骗。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、faithfulness/plausibility definitions、perturbation evaluation 与 discussion；没有把观点写成统一 SOTA 数字。

