# 283. Do I Know This Entity? Knowledge Awareness and Hallucinations in Language Models

- **Authors:** Javier Ferrando, Oscar Obeso, Senthooran Rajamanoharan, Neel Nanda
- **Venue / year:** ICLR 2025
- **Paper:** https://arxiv.org/abs/2411.14257
- **Tags:** Knowledge Awareness, Hallucination, Entity Recognition, SAE Steering

## Introduction

模型是否“知道自己不知道”某个实体，是 hallucination 与 refusal 的关键。论文研究模型在读取 entity 最后 token 时是否形成 recognition direction，区分 known/unknown athlete、movie、city 等实体，并问这些方向只是相关信号还是会因果影响 chat model 的回答。

## Method / Framework

作者使用 Gemma Scope SAEs（Gemma 2 2B/9B）和 Llama Scope（Llama 3.1 8B）观察 entity-final-token 的 latent activation；跨实体类型寻找 known/unknown entity latent，而不是为每个名字单独训练 classifier。随后用 steering 将 known/unknown direction 加回 chat model，测试 refusal/hallucination；还比较 base-model SAE 与 chat fine-tuning 后行为，研究 fine-tuning 是否 repurpose 既有 mechanism。

## Baselines / Comparisons

比较 known vs unknown entities、不同实体类别、Gemma/Llama 模型和 base/chat checkpoint；还对比普通 entity token features、SAE latent classifier 与直接回答行为。内部 knowledge awareness 与外部事实 accuracy 分开评价，避免把“模型拒答”当作“模型真的不知道”的唯一证据。

## Experiments / Findings

SAE latents 能跨电影、城市、歌曲、运动员等类型检测模型是否有可回忆事实的 entity representation。一个 unknown-entity latent 在 Gemma 2B IT 上区分正确/错误回答达到 AUROC 0.732、F1 72；相关 direction steering 可以让模型对 known entity 更愿意回答，或对 unknown entity 更倾向拒答/改变 hallucination 行为。即使 SAE 在 base model 上训练，方向仍能因果影响 chat model refusal，支持“fine-tuning repurposes existing mechanism”。

## Ablation / Error Analysis

跨实体类型和不同模型复现是关键对照；只在单一名字上训练会混淆 lexical identity 与 knowledge awareness。steering strength、层、SAE dictionary 和 prompt 会改变 refusal/生成，且 unknown latent 的语义可能包含 uncertainty、undisclosed information，而非纯粹“无知识”。模型可能知道实体但没有正确事实，或有事实却因 safety/uncertainty 拒答。

## Limitations

“知道”由回答正确性和 latent activation proxy 定义，不等于完整的 metacognition；事实 benchmark 覆盖有限，entity popularity 和训练数据泄漏可能影响结果。steering 会改变行为但不能证明该方向是自然生成电路的唯一必要部分。

## 两句话总结

论文用 SAE 找到跨实体类型的 known/unknown recognition directions，并通过 steering 证明它们会影响 chat model 的知识拒答与 hallucination。结果支持 LLM 存在某种可操纵的 knowledge awareness，但“无知识、低置信度、安全拒答”仍需进一步分离。

## Evidence note

本笔记逐段核对 ICLR/arXiv PDF 的 SAE setup、entity recognition、Table 1-3、steering intervention 和 conclusion。

