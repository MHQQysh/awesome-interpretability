# 217. Visual Abductive Reasoning

- **Authors:** Chen Liang, Wenguan Wang, Tianfei Zhou, Yi Yang
- **Venue:** CVPR 2022
- **Paper:** https://doi.org/10.1109/CVPR52688.2022.01512
- **Tags:** Visual Abduction, Video Reasoning, Causal Structure, Explanation

## Introduction

现有视频 caption 多描述“看到了什么”，却很少从不完整事件推断“最可能发生了什么来解释这些观察”。Visual Abductive Reasoning (VAR) 把这一能力单独定义出来：给定部分可见的视觉事件，模型既要描述 premise，又要生成能解释它的隐藏 hypothesis。

## Method / Framework

作者构建大规模 VAR 数据集，从 YouTube、电影和电视视频中抽取有因果关系的事件并隐藏其中一个 explanation event。REASONER 是 causal-and-cascaded reasoning Transformer：contextualized directional position embedding 编码事件方向/上下文，多个 decoder 先生成 premise 再逐步生成 hypothesis，并用句子 prediction score 引导跨句信息流。

## Baselines / Comparisons

在 VAR test 上比较传统 dense video captioning/video-language models，并报告 premise description 与 invisible explanation hypothesis 的 BLEU-4、CIDEr、METEOR、ROUGE-L、BERTScore。还做 10 人 human study、全部事件可见的 caption 上限、仅 premise text 的语言-only 设置，以及 dense video captioning transfer。

## Experiments / Findings

- REASONER 超过多种视频语言 baseline，在 premise 与 hypothesis 两部分都更好，且从可观察描述到隐藏解释的性能下降更小；在 dense captioning 上相对 SOTA 有约 +2.81 CIDEr。
- 人类能写出合理 hypothesis，但模型与人类仍有明显差距；仅给 premise text 时效果变差，证明视觉证据对 abductive reasoning 不可替代。
- 全部事件可见时分数更高，说明困难确实来自隐藏事件的推断而非简单 caption；causal position encoding 与级联 decoder 的组合支持逐步解释。

## Ablation / Error Analysis

消融 direction position embedding、cascaded decoders、confidence-guided information flow 和 premise/hypothesis 输入。没有因果方向时模型更像按时间复述，只有语言描述会缺少视觉关系；事件过长、多个合理解释、数据偏见和视频遮挡会造成“合理但非唯一”的错误。

## Limitations

abduction 本身允许多个 plausible hypothesis，单一 reference 与 n-gram 指标无法完整评价解释的合理性。VAR 数据的事件划分和因果标签来自人工设计，不能涵盖现实世界所有常识/反事实；模型仍远低于人类，尚未处理长视频实时推理。

## 两句话总结

VAR 将视觉理解从观察描述推进到对隐藏事件的最佳解释，REASONER 用因果位置编码和级联 decoder 生成 premise-to-hypothesis 轨迹。模型优于视频语言 baseline 但仍与人类有明显差距，说明视觉 abductive reasoning 是独立且未解决的能力。
