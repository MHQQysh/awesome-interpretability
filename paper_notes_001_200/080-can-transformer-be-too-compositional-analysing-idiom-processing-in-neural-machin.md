# 080. Can Transformer be Too Compositional? Analysing Idiom Processing in Neural Machine Translation

- **作者 / venue**：ACL 2022
- **论文**：[ACL Anthology](https://aclanthology.org/2022.acl-long.252/)

## Introduction / Method

idiom 的意思不能由词面组合得到，NMT 却常把 idiom 当成 compositional phrase，产生 literal translation。论文比较 literal/figurative idiom 的 attention 和 representation，使用 probing 与 amnesic intervention，检查模型是否具有 idiomatic signal。

## Baseline 与对比

比较 PIE idioms、literal paraphrases、上下文条件、不同语言和模型层；在 translation success/BLEU、attention change 与 probing accuracy 间对照。

## Findings

Transformer 常偏向 compositional processing，导致 figurative idiom 被翻译成字面含义。probe 可以读出部分 idiom information，但 intervention 显示模型是否使用该信息取决于 context；有些 idioms 能独立识别，有些必须依赖上下文。

## 局限

idiom 数据量和语言覆盖有限；翻译错误可能来自词汇、数据或 decoder，而非单纯 compositionality。需要更强的 causal patching 和对比语料。

**一句话评价**：论文展示了“模型能表示 idiom 信息”与“模型在翻译时真正使用 idiomatic meaning”之间的差距。
