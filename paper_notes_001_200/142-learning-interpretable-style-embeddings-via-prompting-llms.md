# 142. Learning Interpretable Style Embeddings via Prompting LLMs

- **Authors:** Ajay Patel, Delip Rao, Ansh Kothary, Kathleen McKeown, Chris Callison-Burch
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.1020
- **Tags:** Style Representation, Synthetic Data, Interpretability, Stylometry

## Introduction

作者指出，style representation 对作者归因、风格迁移和法证语言学很重要，但大规模文本缺少细粒度 stylometric annotation。无监督神经 style vector 虽然有效，却无法告诉用户每一维代表什么，难以审计，也不适合需要可解释性的 authorship attribution。

## Method / Framework

论文用 GPT-3 zero-shot prompting 给 Reddit 文本生成风格描述，再把自然语言描述转换为 768 个可解释 style attributes，形成约 5.5M 样本的 STYLEGENOME。每一维对应 grammar、formality、vocabulary、punctuation、emoji、positivity 等属性，值域是 [0,1]，可读成该风格在文本中存在的概率。

再用这些合成标注训练 LISA（Linguistically Interpretable Style Attribute）embedding。模型不是把整段文本压成黑盒向量，而是输出带属性名称的 bottleneck vector，并可在句子级别或文档级别展示最高激活维度。

## Baselines / Comparisons

比较人工 / 频率特征、黑盒 unsupervised style embedding、EncT5 等神经表示，以及 LISA 的不同数据规模、GPT-3 prompt 组合和属性集合。评测包括 S FAM 与人类对风格判断的一致性、STEL / STEL-or-Content 风格相似度，以及 contrastive authorship verification。

## Experiments / Findings

- STYLEGENOME 让模型可以在大规模、跨主题文本上学习显式风格属性，而不是把 topic 和 style 混在一个 latent vector 中。
- LISA 的 style dimensions 能生成可读解释，例如 conversational、informal、短句、contraction、emoji 或积极语气；句子级向量还可定位长文本中风格变化的位置。
- 在 STEL 等风格评测中，LISA 取得有竞争力的准确率；使用更多且更丰富的合成数据通常优于小数据训练。
- LISA 在 contrastive authorship verification 中同时提供作者判别和触发判别的属性，优于更 opaque 的表示，说明 bottleneck 的解释性可以转化为下游可用性。
- GPT-3 的 annotation 会 hallucinate 属性、误读文本或混入内容主题；作者通过多 prompt 和一致性过滤减少错误，但不能完全替代人工标注。

## Ablation / Error Analysis

主要消融是 STYLEGENOME 数据量、Stage 1 prompt 组合、属性 bottleneck 大小和是否保留内容相关属性。Prompt 多样性改善 S FAM 与人类判断的一致性；过度依赖少数 handcrafted dimensions 会限制模型表达。错误案例表明，模型会把“文本讨论某个主题”误当成“作者具有某种风格”，或为没有出现的 punctuation / emoji 生成高分。

## Limitations

合成标注继承 GPT-3 的偏见、幻觉和风格观念，768 维属性仍是作者预先定义的观察空间。Reddit 文本和英文分布限制了跨域、跨语言结论；style 与 content 并不能总被清楚分离。法证和招聘等高风险场景仍需要专家复核，不能把 LISA 分数当成作者身份的决定性证据。

## 两句话总结

论文用 LLM 生成大规模可读 stylometric labels，再蒸馏出每一维有明确含义的 LISA style embedding。它把“风格向量能不能解释”变成可检查的属性预测问题，但解释质量仍受 GPT-3 合成标注和预定义属性集合限制。
