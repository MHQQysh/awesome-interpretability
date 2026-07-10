# 174. Comparing Explanation Faithfulness between Multilingual and Monolingual Fine-tuned Language Models

- **Authors:** Zhixue Zhao, Nikolaos Aletras
- **Venue:** NAACL 2024
- **Paper:** https://aclanthology.org/2024.naacl-long.178
- **Tags:** Multilingual, Faithfulness, Feature Attribution, Cross-lingual

## Introduction

feature attribution 和 rationale 常被当成模型解释，但其 faithfulness 可能受语言、模型和 fine-tuning 方式影响。过去多研究英语单语模型，尚不清楚 multilingual XLM-R 与单语 RoBERTa 在同一任务上的 explanation 是否同样忠实。

## Method / Framework

作者在五种语言、五个任务上比较 multilingual 与 monolingual fine-tuned models，对 attention、gradient / input attribution 等解释做 faithfulness tests，例如 input removal、sufficiency、comprehensiveness 和 feature perturbation。预测性能与解释 faithfulness 分开报告。

## Baselines / Comparisons

比较 XLM-R 与各语言 RoBERTa / 单语模型、不同 FA 方法、不同 task / language 和不同 fine-tuning 数据量。baseline 包括随机 / uniform attribution 与 prediction-preserving deletion。

## Experiments / Findings

- multilingual 和 monolingual 模型的 accuracy 差异并不能直接预测 explanation faithfulness；同一 FA method 在不同语言上的排名会改变。
- 单语模型有时提供更集中、更 faithful 的 features，但 multilingual 模型在低资源语言上可能依靠跨语共享表示获得更好泛化。
- attention heatmap 不总是最 faithful，梯度 / perturbation 结果依赖 baseline、tokenization 和语言形态。
- 语言间差异来自数据、词形、句法和模型容量的共同作用，而非简单的“多语模型更不透明”。

## Ablation / Error Analysis

消融语言、模型、task、FA method、删除比例和 baseline。子词分割、形态丰富语言、输入长短和 label imbalance 会改变 attribution；只报告一层或一个删除比例容易得出相反结论。

## Limitations

五种语言和有限任务不能代表所有 multilingual setting；faithfulness metric 仍是 proxy。删除 token 可能改变输入分布，不能完全模拟真实 causal intervention；结果也不直接适用于生成式 LLM rationale。

## 两句话总结

论文显示多语与单语模型的解释忠实性需要单独测量，不能从 accuracy 或模型语言数目推断。FA 方法、tokenization、语言和删除协议共同决定结论，因此跨语言解释研究必须报告完整 faithfulness protocol。
