# 164. FLUTE: Figurative Language Understanding through Textual Explanations

- **Authors:** Tuhin Chakrabarty, Arkady Saakyan, Debanjan Ghosh, Smaranda Muresan
- **Venue:** EMNLP 2022
- **Paper:** https://aclanthology.org/2022.emnlp-main.481
- **Tags:** Figurative Language, NLI, Explanation, Dataset

## Introduction

figurative language 的 NLI benchmark 容易含有 spurious correlations，模型能猜对 entail / contradict 却不理解 sarcasm、simile、metaphor 和 idiom。已有 e-SNLI 有解释，但缺少针对 figurative language 的 explanation dataset。

## Method / Framework

FLUTE 包含 9,000 个 figurative NLI instances，覆盖 Sarcasm、Simile、Metaphor、Idioms；每项有 literal / figurative sentence pair、entail / contradict label 和自然语言 explanation。数据用 GPT-3 model-in-the-loop 生成候选，再由 crowd workers 和 expert annotators 修订、过滤和保证质量。

用 T5 fine-tune 同时做 NLI label 与 explanation generation，测试 textual explanation 是否能让模型 right for the right reasons。

## Baselines / Comparisons

比较只预测 NLI label 的 T5、带 explanation multitask T5、不同 figurative category 和人工 / GPT 生成数据。分析与 e-SNLI、其它 figurative NLI benchmark 的 label 预测和 explanation quality。

## Experiments / Findings

- T5 在 FLUTE 上能学到 figurative entailment / contradiction，但性能明显低于普通 NLI 的简单 in-domain 结果，说明 figurative understanding 更难。
- explanation supervision 让模型输出更接近 literal-to-figurative 语义桥接，而不是只依赖表面词。
- GPT-3 + novice + expert 的协作能扩展复杂 figurative annotations；expert filtering 对解释完整性和隐含语义很重要。
- 不同类别表现不同：idiom / metaphor 需要背景知识和隐式映射，sarcasm 更依赖语气与社交上下文。

## Ablation / Error Analysis

消融 explanation supervision、category、GPT-3 candidate 与人工修订。去掉 explanation 会保留高 label accuracy 但降低 right-reason 可信度；错误主要来自 figurative expression 被 literal 解读、缺少文化知识、以及 premise / hypothesis 的关系不止一跳。

## Limitations

GPT-3 生成候选可能引入模板偏差和 hallucination，crowd / expert 标注成本仍高。9,000 条数据覆盖四类英文表达，不能代表跨文化、跨语言 figurative language；NLI label 和 explanation 的一致性也不等于模型内部真正理解。

## 两句话总结

FLUTE 将 figurative NLI 与 textual explanations 结合，使模型不仅要判断 entailment，还要说明 literal 与 figurative meaning 如何连接。数据和 T5 baseline 证明解释监督有价值，但隐含文化知识、类别差异和数据生成偏差仍限制泛化。
