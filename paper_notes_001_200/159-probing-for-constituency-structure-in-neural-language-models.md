# 159. Probing for Constituency Structure in Neural Language Models

- **Authors:** David Arps, Younes Samih, Laura Kallmeyer, Hassan Sajjad
- **Venue:** EMNLP Findings 2022
- **Paper:** https://aclanthology.org/2022.findings-emnlp.502
- **Tags:** Probing, Constituency, Syntax, Selectivity

## Introduction

作者研究 contextual LM 是否隐式学习 constituency，而不是只依赖 semantic correlation。PTB constituency tree 比 dependency tree 包含更细粒度层级结构；论文用诊断 classifier 检验模型 activation 中是否能恢复 constituent category 和完整树。

## Method / Framework

对两个 token 的 contextual representation 做 concatenation、average 或 max pooling，训练简单线性 probe 预测它们最低共同祖先（LCA）和 constituent label；同时做 chunking、full parse reconstruction 和 layer / neuron analysis。

为区分 syntax 与 semantics，作者构造 nonce PTB：随机替换 constituent，使句子语义不自然但保留句法结构。还用 control task、selectivity、随机 / top neuron / bottom neuron 对照，检查 probe 是否只是利用低级词面特征。

## Baselines / Comparisons

模型包括 BERT、RoBERTa、DistilBERT、XLNet 等 12-layer pretrained LM。比较 LCA / chunking / full tree probe、concat / avg / max 表示、原始 PTB 与 nonce PTB、随机 control、dependency bracket baseline 和不同 layer / neuron 子集。

## Experiments / Findings

- 四个 transformer LM 在 nonce data 上仍获得较高 probing performance，支持模型确实编码了句法而非只记住语义。
- concatenating 两个 token representations 明显优于 averaging / max；LCA 的 overall accuracy 约 82.8% 的结果展示了组合表示的优势。
- 低层和中层通常更适合细粒度 syntax；不同模型把 syntax 与 semantic information 分布在不同 neuron / layer。
- 线性 probe 可以重建完整 constituency tree，最佳 labeled F1 达到 96.3，说明完整树在表示中具有线性可分结构。
- 需要大量训练数据才能达到高 probe performance；在原始数据上高分并不自动证明 probe 学到的是模型可泛化的句法机制。

## Ablation / Error Analysis

nonce replacement、control task、pooling 方法、layer、neuron fractions 和 PTB tagset 是主要消融。top neurons 不总是比随机 neuron 好，说明单纯按 probe weight 做 neuron attribution 可能不稳定。少数 constituent、长距离 token 和细粒度标签更难；不同模型的 neuron overlap 较低，表示句法编码位置并不统一。

## Limitations

probe 仍可能学习到表示中的相关性，不能直接证明 LM 在生成时使用这些 constituent。PTB 英语树库、人工 nonce 数据和 linear classifier 限制了跨语言结论。完整 parse reconstruction 数据量大且计算昂贵，实际大模型的层数和 hidden size 可能带来新的 probe capacity 问题。

## 两句话总结

论文用 nonce 语料和 selectivity 控制证明，神经语言模型的表示中确实包含可线性恢复的 constituency structure，而不只是语义相关性。拼接 token 表示和全树 probe 很强，但 probing 结果仍需要因果干预才能说明模型实际依赖了这些句法表示。
