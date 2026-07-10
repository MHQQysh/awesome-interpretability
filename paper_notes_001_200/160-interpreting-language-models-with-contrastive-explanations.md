# 160. Interpreting Language Models with Contrastive Explanations

- **Authors:** Kayo Yin, Graham Neubig
- **Venue:** EMNLP 2022
- **Paper:** https://aclanthology.org/2022.emnlp-main.14
- **Tags:** Contrastive Explanation, Attribution, Simulatability, GPT-2

## Introduction

传统 Gradient × Input 等 salience map 常把“为什么生成 barking”归因给紧邻的 dog / from，却不能回答“为什么是 barking 而不是 crying 或 walking”。语言模型词表很大，一个 token 同时受词性、时态、语义和上下文影响，因此单一非对比解释把多个决策混在一起。

## Method / Framework

作者将 Gradient × Input、Gradient Norm 和 Input Erasure 扩展为 contrastive explanations：给定 target token yt 与 foil token yf，解释哪些 input token 提高 yt 相对 yf 的概率。非对比方法解释 target 的绝对 salience；对比方法解释 logit / probability difference。

实验在 GPT-2 和 GPT-Neo 上使用 BLiMP minimal pairs 及 WikiText-103，计算 explanation 与已知 grammar evidence 的 dot product、probes needed 和 MRR。另做 10 名研究生、4,000 个 model-simulatability 样本的人类实验，并对 foil explanation 聚类，发现模型在不同词对中重复使用的证据。

## Baselines / Comparisons

比较 None、Gradient × Input（GI）、Gradient Norm（GN）、Input Erasure（E）及各自 contrastive 版本；另有 random salience baseline。模型输出正确 / 错误、evidence 距离、POS / grammar paradigm 都分别报告。

## Experiments / Findings

- Contrastive explanations 在大多数 BLiMP grammar phenomena 上比对应 non-contrastive explanation 更贴近已知 evidence；普通 salience 有时甚至不如 random，但 contrastive 版本大多超过 random。
- GPT-2 的 contrastive GI 在若干指标上达到 dot product 0.50、probes needed 1.33、MRR 0.61，优于非对比 GI 0.36 / 1.44 / 0.59；contrastive erasure 的 MRR 也更高。
- 证据离 target 越远，contrastive 相对 non-contrastive 的 MRR 增益通常越大，说明对比解释更适合长程依赖。
- 人类模拟 GPT-2 输出时，无解释准确率 61.38%；contrastive GI 65.62%，contrastive erasure 64.62%，高于对应非对比版本。参与者也更常认为 contrastive explanation 有用。
- foil clustering 能发现多个词对共享的 linguistic context，而不是只给单个实例热力图。

## Ablation / Error Analysis

target / foil 选择、GI / erasure、GPT-2 / GPT-Neo、正确 / 错误预测和 evidence distance 是主要消融。若输入中没有区分两个词的证据，所有解释方法都难；“son / brother”“fast / super”等高度可互换词对的人类模拟准确率最低。输入 erasure 计算昂贵，梯度解释更易扩展但可能受 baseline 影响。

## Limitations

实验只覆盖 GPT-2 / GPT-Neo、部分 BLiMP grammar phenomena 和英文 next-token prediction。contrastive salience 仍是 post-hoc attribution，不保证是模型内部真实因果路径；人类 simulatability 受参与者语言知识和 foil 难度影响。生成式任务中 target / foil 的选择本身也会改变解释结论。

## 两句话总结

论文把语言模型解释从“为什么生成这个词”改成“为什么生成这个词而不是另一个合理词”，使 grammar 和长程 evidence 更可见。对比解释在自动对齐和人类模拟上都优于非对比 salience，但仍需因果干预和更大模型验证。
