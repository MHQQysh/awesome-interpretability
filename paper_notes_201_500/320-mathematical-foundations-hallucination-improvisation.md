# 320. Mathematical Foundations of Hallucination in Transformer-Based Large Language Models for Improvisation

- **Authors:** Ahmet Gundogmusler, Fatma Bayindiroglu, Mustafa Karakucukoglu
- **Venue / year:** 2024 preprint
- **Paper:** https://www.techrxiv.org/doi/pdf/10.36227/techrxiv.171925719.97800486
- **Tags:** Hallucination Theory, Transformer Mathematics, Improvisation, Generative Models

## Introduction

开放式 improvisation 需要模型在已有 pattern 上生成新组合，但 transformer 的概率生成、attention、sampling 和训练分布缺口也会产生无依据内容。本文尝试给出 hallucination 的数学表述，连接模型分布、context、uncertainty 与创作性生成。

## Method / Framework

工作以 transformer 的 conditional token distribution、attention/representation 和 sampling dynamics 为基础，讨论 hallucination 何时表现为 generated sequence 与 learned/ground-truth distribution 的偏离。improvisation 场景允许一定 novelty，因此框架区分 creative variation 与 factual inconsistency，并用数学符号说明二者边界。

## Baselines / Comparisons

主要比较 deterministic/temperature sampling、grounded factual generation 与 improvisational generation 的目标；它不是单一 detector benchmark。评价轴包括 likelihood/uncertainty、novelty、coherence、factual alignment 和 human judgment。

## Experiments / Findings

理论主张 hallucination 与模型不确定性、训练分布外输入、attention/context limitations 和 sampling temperature 相关；在创作任务中部分偏离可能是有价值的 improvisation，但在事实任务中需要 grounding/verification。数学框架的意义是让“创造性”和“错误”在定义上分开，而非简单惩罚所有新颖 token。

## Ablation / Error Analysis

temperature、context length、training coverage、retrieval evidence 与 task definition 是关键变量；高 novelty 可能看似创意却缺乏 coherence，低 novelty 也可能复述错误记忆。理论量需要与 human ratings、source checking 和真实任务结果对照。

## Limitations

TechRxiv 预印本的理论定义与实际 hallucination detector 之间仍有距离；“improvisation value”难以客观量化，开放域 ground truth 也不唯一。不能把形式推导直接当作临床/法律可靠性保证。

## 两句话总结

本文从 transformer 概率生成和 sampling 的数学视角讨论 hallucination，试图区分事实错误与 improvisation 中有价值的创意偏离。它提供了统一讨论语言，但 novelty、coherence 与 factuality 的实际度量仍需任务化实验和人类评价。

## Evidence note

本笔记核对 TechRxiv 预印本题录、方法/理论描述和讨论；未将无法逐表复核的数值写入。

