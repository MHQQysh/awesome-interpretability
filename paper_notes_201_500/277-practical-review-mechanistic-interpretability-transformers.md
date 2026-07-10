# 277. A Practical Review of Mechanistic Interpretability for Transformer-Based Language Models

- **Authors:** Daking Rai, Yilun Zhou, Feng Shi, Abulhair Saparov, Ziyu Yao
- **Venue / year:** 2024 survey
- **Paper:** https://arxiv.org/abs/2407.02646
- **Tags:** Mechanistic Interpretability Survey, Circuits, SAEs, Roadmap

## Introduction

MI 文献已经覆盖 neurons、features、circuits、attention、SAE、activation patching 等大量工具，但新人很难知道每个方法回答什么问题、需要何种证据、下一步如何复现。本文以 task-centric taxonomy 组织 transformer LM 的 MI，目标是给出从观察 feature 到定位/解释 circuit 的实践路线。

## Method / Framework

综述先介绍 transformer residual stream、attention/MLP、features 与 circuits，再把研究任务分成 observe/interpret features、localize circuits、explain algorithms、evaluate interventions 等阶段。它对 vocabulary projection/logit lens、activation patching、path patching、ablation、SAE、probing 和 automated interpretability 做概念-技术-评价三层整理，并给出 beginner roadmap 和代表性 circuits/neurons 表格。

## Baselines / Comparisons

这是一篇 survey，不做单一新模型实验；比较轴是方法适用的 target/open-ended feature study、causal localization 与 circuit interpretation，及其需要 black-box/white-box access、单样本/多样本、局部/全局解释的差异。作者也把它与已有 interpretability survey 对照，突出“按任务组织”而非只按方法名称罗列。

## Experiments / Findings

综述归纳的共识是：logit lens/attention/probing 适合发现候选 feature，但 causal patching/ablation 才能验证功能；SAE 提供可解释特征词表却有 superposition、feature splitting/occlusion；IOI、induction heads、name mover 等 circuits 是算法级 MI 的典型案例。研究流程应从任务定义、组件定位、机制解释、必要性/充分性测试逐级推进。

## Ablation / Error Analysis

主要风险包括把 attention weight 当作 causal importance、把 probe accuracy 当作 model usage、把单一 ablation 当作完整 circuit，以及忽视 distribution shift 与 feature reconstruction error。综述建议报告 control/negative controls、faithfulness、robustness、human interpretability 和可复现实验，而不是只展示漂亮 activation 图。

## Limitations

综述不能替代逐篇阅读，taxonomy 会随 automated circuit discovery、attribution graphs 和大模型实验快速变化；各研究的 metric/protocol 不统一，跨论文数值比较有风险。roadmap 主要面向 decoder transformer，非 transformer/多模态模型覆盖有限。

## 两句话总结

这篇 practical survey 给出了从 feature 观察、因果定位到 circuit 解释和评估的任务型路线，适合把 MI 文献组织成可执行工作流。它反复强调候选相关性不等于机制证明，真实进展需要干预、负对照、鲁棒性和跨分布验证共同支撑。

## Evidence note

本笔记逐段核对 survey 的 taxonomy、Tables 1-10、technique/evaluation roadmap 与 open problems；没有把综述中的代表性案例误写成作者新实验。

