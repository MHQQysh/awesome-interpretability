# 158. “Will You Find These Shortcuts?” A Protocol for Evaluating the Faithfulness of Input Salience Methods for Text Classification

- **Authors:** S. Feng, Jordan Boyd-Graber and collaborators
- **Venue:** EMNLP 2022
- **Paper:** https://aclanthology.org/2022.emnlp-main.64
- **Tags:** Faithfulness, Salience, Shortcut, Evaluation

## Introduction

输入 salience 方法常被用来解释文本分类器，但一个显眼 token 不一定是模型真正依赖的 causal evidence。作者关注 shortcut：模型可能靠评分数字、固定 token 顺序或数据 artifact 做决定；如果 salience 排名找不到这些 shortcut，解释就不 faithful。

论文提出协议，让研究者先提出模型可能使用的 shortcut，再构造保持标签但只改变 shortcut 的数据，测试 attribution 是否把 shortcut token 排到前面。它的核心警告是：salience 方法的配置（L1 / dot product、logit / probability、baseline）会显著改变结论。

## Method / Framework

对每个 classifier / dataset，协议包括：

1. 定义可验证的 shortcut hypothesis；
2. 构造合成或自然文本变体，控制真正任务证据与 shortcut；
3. 用 salience map 对 token 排名；
4. 用 precision、rank、deletion / insertion 或 shortcut recovery 检查解释是否找到了预先知道的证据。

论文系统测试 Gradient × Input、Gradient Norm、Integrated Gradients 和 LIME，并在 LSTM / BERT 上比较各种 baseline token masking、reference embedding、logit / probability 等配置。

## Baselines / Comparisons

比较四类 salience 方法、不同模型、不同 shortcut 类型、不同 mask token 和不同 attribution baseline。每种配置同时用原始数据与人工构造 shortcut data，避免把单一方法在单一设置上的高分误当成通用 faithfulness。

## Experiments / Findings

- salience 方法能否找到 shortcut 强烈依赖任务和配置，没有一个方法在所有数据集上稳定最好。
- Gradient × Input、IG、LIME 等在 BERT 与 LSTM 上的排名不同；把 logits 换成 probabilities、把 UNK 换成 MASK、改变 IG baseline 都可能改变结果。
- 某些被文献认为次优的配置，在揭示 lexical shortcut 时反而更好；因此复现解释性结论必须记录完整 configuration。
- 任务性能高不代表 salience faithful：模型可能靠一个 shortcut 得到高 accuracy，而解释方法仍把内容词排在前面。

## Ablation / Error Analysis

主要消融是 shortcut 类型、masking token、baseline embedding、目标输出、L1 / dot-product aggregation 和模型架构。错误来自 shortcut 不可识别、多个 token 的组合效应、删除 token 改变了输入分布，以及 salience 只反映局部梯度而不是全局因果依赖。作者建议先为每个任务写出 shortcut 假设，再选择能检验它的 attribution 配置。

## Limitations

协议依赖研究者事先提出 shortcut，无法发现完全未知的模型机制。实验主要覆盖文本分类、常见 salience 方法和有限模型；真实语言中的长程交互、多 token shortcut 和生成任务需要额外设计。faithfulness 指标也可能被合成数据的人工结构影响。

## 两句话总结

论文把 salience faithfulness 变成“是否能找回预先构造的 shortcut evidence”的可检验协议，而不是只展示漂亮热力图。结果说明方法和配置差异足以反转结论，解释性实验必须同时报告 shortcut hypothesis、masking、baseline 和模型细节。
