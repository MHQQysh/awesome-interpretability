# 312. A Survey on Large Language Model Hallucination via a Creativity Perspective

- **Authors:** Xuhui Jiang, Yuxing Tian, Fengrui Hua, Chengjin Xu, Yuanzhuo Wang, Jian Guo
- **Venue / year:** 2024 survey
- **Paper:** https://arxiv.org/abs/2402.06647
- **Tags:** Hallucination, Creativity, Evaluation, Safety

## Introduction

通常把 hallucination 视为事实错误，但开放式写作、故事和 ideation 又需要生成训练语料中没有的内容。本文的问题是 hallucination 是否总是有害，能否从 creativity 视角区分有价值的新颖性与需要抑制的无依据事实。

## Method / Framework

survey 从 hallucination definition/taxonomy、causes、detection/mitigation 和 creativity relationship 组织文献，把 novelty、surprise、usefulness 与 factual grounding 放到同一讨论框架。它区分 creative generation 场景和 legal/medical/financial 等 factual-critical 场景，强调评估标准必须随任务改变。

## Baselines / Comparisons

比较 factuality/faithfulness、novelty/diversity、human creativity judgment、retrieval/verification 和 self-consistency 等评价/缓解方法。survey 不提出新模型，主要比较“降低 hallucination”与“保留探索性”之间的目标冲突。

## Experiments / Findings

核心观点是同一生成行为在故事创作中可能是 creative deviation，在事实问答中却是 harmful hallucination；因此 blanket suppression 可能牺牲创新。可靠系统应识别任务模式、给出 uncertainty/source、在高风险场景偏向 grounding，在开放创作场景允许受控新颖性。

## Ablation / Error Analysis

prompt、temperature、domain、source availability 和 human evaluator 价值观会改变 hallucination/creativity 判断；只看 factual accuracy 会惩罚合理创意，只看 novelty 会奖励胡编。需要同时测 usefulness、coherence、groundedness 和 user intent。

## Limitations

creativity 本身难以客观定义，survey 的跨任务比较不统一；创意与事实的边界也随应用、用户和文化变化。它提供的是评价哲学与分类，不是可直接部署的 detector。

## 两句话总结

本文提醒 hallucination 不是一个脱离任务的单一坏现象：事实场景要压制，创作场景可能是新颖性来源。真正的系统应根据用户意图和风险在 grounding 与探索之间调节，而不是用一个指标消灭所有偏离。

## Evidence note

本笔记逐段核对 survey 的 creativity framing、taxonomy、evaluation/mitigation discussion 与 limitations。

