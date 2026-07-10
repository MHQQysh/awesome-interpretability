# 267. Precise In-Parameter Concept Erasure in Large Language Models

- **Authors:** Yoav Gur-Arieh, Clara Haya Suslik, Y.-W. Peter Hong, Fazl Barez, Mor Geva
- **Venue / year:** 2025 preprint
- **Paper:** https://arxiv.org/abs/2505.22586
- **Tags:** Concept Erasure, In-Parameter Editing, Sparse Features, Model Utility

## Introduction

模型编辑可以删除敏感信息、版权概念或不希望下游模型生成的知识，但 fine-tuning、low-rank adapter 和 fact-level editing 往往过于粗糙：要么擦掉相关领域能力，要么只浅层压低记忆，重训后又恢复。PISCES 的问题是能否在参数空间识别真正编码目标概念的方向，只编辑这些参数并保持无关能力。

## Method / Framework

PISCES (Precise In-parameter Suppression for Concept Erasure) 先用 disentangler 把 MLP vectors 分解成较可解释的 features，再通过 automated interpretability 为 feature 赋 alignment/coherence，识别与目标 concept 相关的 features。随后在参数中删除/编辑这些方向，reconstruct 原参数并 replace 回模型，而不是只在输出端加 adapter。评价同时看 efficacy（目标概念准确率下降）、specificity（相近概念与无关知识保留）、coherence、robustness（relearning 后能否恢复）和普通指令能力。

## Baselines / Comparisons

实验在 Gemma-2-2B-it 与 Llama-3.1-8B-it 上比较 PISCES、RMU、ELM，并包括 MEMIT、AlphaEdit 等编辑方法作为参考；概念来自 ConceptVectors benchmark，覆盖 Harry Potter、Ancient Rome、Gambling、Golf、Baseball、Uranium 等 11 类。评价采用目标/相似 domain/open-style questions、Alpaca-Eval coherence、MMLU 和 retraining-on-filtered-text robustness。

## Experiments / Findings

- 摘要报告 PISCES 可把目标概念准确率压到最低 7.7%，specificity 最多提升 31%，robustness 最多提升 38%，说明它不是简单把相关输出整体压低。
- Gemma-2-2B-it 上 Harry Potter 的 target accuracy 约 0.045、Ancient Rome 0.027、Golf/Llama-3.1 约 0.038；普通 Alpaca/MMLU coherence 多接近 1.0 normalized performance，显示无关 instruction 能力较好保留。
- feature-level analysis 发现同一目标可能需要少量 feature，但 alignment/coherence 不总是完美：例如 Pornography/Uranium feature alignment 低，说明 concept direction 可能与更上位或相关概念纠缠，精确擦除与泛化保护存在张力。

## Ablation / Error Analysis

关键消融是 feature selection、disentangle/edit/reconstruct 的各阶段及阈值选择。误选与目标相邻但非目标的 feature 会降低 specificity；编辑太少会留下浅层记忆，编辑太多会伤害 MMLU/domain utility。retraining evaluation 暴露了“suppression 而非 unlearning”：如果只压低生成 logits，模型可从不含直接答案的相关文本重新学回目标概念。LLM-as-a-Judge 的 coherence 也可能掩盖细粒度事实损伤。

## Limitations

PISCES 依赖 disentangler feature 的可解释性与自动 annotation 质量；非 basis-aligned concept、polysemantic feature 和多概念重叠会使参数编辑不稳定。目标概念列表有限，重训攻击与真实 adversarial extraction 仍不同；删除概念也可能与版权、隐私和模型能力边界的治理问题冲突。

## 两句话总结

PISCES 用可解释 feature 分解定位编码目标概念的 MLP 参数方向，再直接编辑并重构这些参数，从而提高 concept erasure 的 specificity 和 relearning robustness。它显示精确擦除比粗粒度 unlearning 更能保留模型能力，但 feature polysemanticity、自动评价和真正不可恢复性仍需谨慎。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、PISCES pipeline、Table 1/4/5/6/7、robustness protocol 与 Analysis；摘要中的最大提升按作者报告保留。

