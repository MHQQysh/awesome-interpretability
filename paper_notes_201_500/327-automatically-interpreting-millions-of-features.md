# 327. Automatically Interpreting Millions of Features in Large Language Models

- **Authors:** Gonçalo Paulo, Alex Mallen, C. H. Juang, Nora Belrose
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2410.13928
- **Tags:** Auto-Interpretability, SAE, Feature Scoring, LLM Explainer

## Introduction

SAE 可以提取 millions of features，但人工逐一阅读激活样本不可行；需要自动生成解释并可靠地评分。论文的问题是如何建立可扩展的 explainer/scorer pipeline，同时避免“解释看起来对”却没有覆盖 feature 真实行为。

## Method / Framework

pipeline 用 LLM 根据 SAE feature 的 activating examples 生成自然语言 interpretation，再用多个 scoring methods 评价：simulation 让 scorer 预测每个 token 的 feature activation；detection 判断上下文是否激活 feature；fuzzing、surprisal、embedding similarity 和 intervention scoring 分别检验 context classification、信息量、语义和行为影响。作者在不同 SAE size、activation function、loss 与两个 open-weight LLM 上比较 prompt design。

## Baselines / Comparisons

比较 LLM explainer、不同 scorer model（如 Llama/Claude）、top/random examples、是否给 non-activating contexts、CoT、训练语料变化和五种 scoring metric。simulation 是传统 baseline，但 detection 更关注 context precision/recall，intervention scoring 则直接测试 feature 对 subject model output 的 counterfactual effect。

## Experiments / Findings

自动 interpretation/scoring 能覆盖大规模 SAE，但不同 metric 衡量不同质量：simulation 可能受 token localization 影响，detection 更宽容；fuzzing/detection 结合可发现解释只抓常见 token、没抓真正 context 的情况；intervention score 能把“相关 feature”与“影响行为的 feature”区分。更大 example set 略提高分数，CoT 对解释质量提升有限却增加显著成本。

## Ablation / Error Analysis

scorer model、activating/non-activating examples、prompt/CoT、interpretation length、intervention strength 是关键消融。强 intervention 可人为制造确定输出，必须固定 KL strength；短解释通常更实用，但 metric 若不惩罚长度会奖励冗余。LLM scorer 自身会带偏差，不能把 automated score 当 ground truth。

## Limitations

百万 feature 的自动解释仍需大量 inference；当前 score 多是 proxy，且不同 SAE/model 分布不一定可比。intervention scoring 需要 subject model access，闭源模型和跨层 circuits 仍困难。

## 两句话总结

论文构建了从 SAE examples 生成自然语言解释、再用 detection/simulation/surprisal/intervention 多指标评分的百万 feature pipeline。它证明自动解释必须多维并加入行为验证，单一 simulation 或漂亮文字不足以衡量 feature interpretability。

## Evidence note

本笔记逐段核对 arXiv PDF 的 interpretation pipeline、五类 scoring、SAE/model experiments、prompt ablations 与 conclusion。

