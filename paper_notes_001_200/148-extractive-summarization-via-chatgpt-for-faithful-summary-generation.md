# 148. Extractive Summarization via ChatGPT for Faithful Summary Generation

- **Authors:** Haopeng Zhang, Xiao Liu, Jiawei Zhang
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.214
- **Tags:** Summarization, Faithfulness, Extractive, ChatGPT

## Introduction

Abstractive summarization 灵活，却容易生成原文没有的事实；extractive summarization 直接选原句，通常更 faithful，但传统监督系统需要标注和训练。论文评估 ChatGPT 能否直接做 extractive summary，并检验少量 in-context / CoT reasoning 是否有帮助；进一步把抽取结果作为 abstractive generation 的 grounding。

## Method / Framework

给 ChatGPT 一个长文档，要求返回最重要的句子；在 zero-shot 外，加入 document-summary exemplars，以及包含 explanation / chain-of-thought 的 input-explanation-output triplets。第二阶段先抽取 salient sentences 得到 sE，再让 ChatGPT 基于 sE 生成 abstractive summary，形成 extract-then-generate。

数据覆盖 CNN/DM、XSum、Reddit 和 PubMed；比较 ROUGE、G-EVAL、FactCC / QuestEval 等自动质量与 factuality 指标，并分析 lead / positional bias。

## Baselines / Comparisons

比较传统监督 extractive SOTA、ChatGPT-Ext、ChatGPT-Abs、ChatGPT-Ext-Abs、Oracle-Abs 和不同 prompt。Oracle-Abs 使用人工 gold summary 选择的理想句子作为输入，用来估计抽取上限。指标同时覆盖 lexical overlap 与 LLM-based quality，避免 ROUGE 单独决定结论。

## Experiments / Findings

- ChatGPT-Ext 的 ROUGE 低于监督 extractive SOTA；在 CNN/DM 上 R1/R2/RL 为 39.25/17.09/25.64，而 SOTA-Ext 为 44.41/20.86/40.55。
- 但 G-EVAL 等 LLM-based 评估中 ChatGPT-Ext 接近或部分超过传统模型，说明人工感知质量与 n-gram overlap 并不一致。
- context exemplars 和 reasoning prompt 在不同数据集上收益不稳定；在 CNN/DM 中 context 提升 R1，reasoning 并非总是带来 ROUGE 提升。
- ChatGPT-Ext-Abs 相比 ChatGPT-Abs 在多个数据集的 factuality 显著改善，抽取句作为 grounding 能减少 abstractive hallucination，并保持或提高摘要质量。
- positional analysis 显示 ChatGPT 的抽取仍存在 lead bias，选出的句子位置分布与理想 ORACLE 不同，不能简单认为“模型理解了全文重要性”。

## Ablation / Error Analysis

抽取 prompt、context examples、CoT reasoning、抽取后生成、文档长度和 lead position 是主要消融。抽取阶段错误会传递给第二阶段；过度压缩会丢掉连接信息，生成阶段仍可能改写出原文不存在的内容。FactCC 对 ChatGPT 生成摘要的分数可能极低，说明自动 factuality evaluator 与 LLM 语言风格存在错配。

## Limitations

主要实验依赖 ChatGPT API，模型版本和 prompt 更新会破坏复现。研究使用自动指标和 G-EVAL，没有完成大规模人工 faithfulness study；作者把人类评测留作 future work。extract-then-generate 不能保证逐句事实一致，只能降低 hallucination 风险。

## 两句话总结

论文发现 ChatGPT 直接做 extractive summarization 的 ROUGE 不如监督模型，但其语言质量评价更有竞争力；先抽取再生成则显著提高 abstractive summary 的 faithfulness。抽取 grounding 是有效的结构约束，却仍受 lead bias、自动评估器和第二阶段幻觉影响。
