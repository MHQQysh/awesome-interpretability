# 181. DecoderLens: Layerwise Interpretation of Encoder-Decoder Transformers

- **Authors:** Anna Langedijk, Hosein Mohebbi, Gabriele Sarti, Willem Zuidema, Jaap Jumelet
- **Venue:** NAACL Findings 2024
- **Paper:** https://aclanthology.org/2024.findings-naacl.296
- **Tags:** Layerwise Interpretation, Encoder-Decoder, Probing, DecoderLens

## Introduction

encoder-decoder Transformer 的 encoder 表示和 decoder generation 之间隔着多层 cross-attention，难以知道模型在中间层已经“知道”什么。DecoderLens 把 decoder 从每个 encoder layer 提前接出，观察中间层可生成的文本和答案质量。

## Method / Framework

对每个 encoder layer 的 hidden states，接同一个 decoder / language head 进行 early decoding，比较 layer-wise output、token probability、sequence quality 和 task prediction。无需重新训练模型，直接把模型的内部表示转换成可读文本。

## Baselines / Comparisons

比较不同 encoder layer、最终 encoder-decoder 输出、随机 / input baseline 和不同 encoder-decoder 架构。指标包括生成质量、校准、词汇 / 句法信息和下游任务准确率。

## Experiments / Findings

- 中间层已经逐渐形成可读、与输入相关的 output，层越深通常越接近最终答案。
- 不同层承担不同抽象：低层更偏词汇和局部信息，高层更偏任务相关语义与完整生成。
- layerwise decoding 能定位错误首次出现的位置，并区分 encoder 没编码信息与 decoder 没利用信息。

## Ablation / Error Analysis

消融 decoder head、layer、beam / greedy decoding 和 task。中间层输出可能语法流畅却事实不完整，不能把可读性直接等同于内部机制；不同 decoder 参数共享也会影响层间可比性。

## Limitations

early-exit 文本是 probe-like readout，不是模型自然会生成的路径；只覆盖 encoder-decoder 架构。生成质量受 decoder head 和 decoding strategy 影响，不能直接证明某一层有独立语义模块。

## 两句话总结

DecoderLens 通过从每个 encoder layer 提前接出 decoder，把 encoder-decoder 的表示演化转成可观察文本。它能定位知识和错误何时出现，但 layerwise generation 仍是外部读出，不等于完整因果解释。
