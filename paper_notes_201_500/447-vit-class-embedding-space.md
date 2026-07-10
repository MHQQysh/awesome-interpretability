# 447. Analyzing Vision Transformers for Image Classification in Class Embedding Space

- **Authors:** Martina G. Vilas, Timothy Schäumlöffel, Gemma Roig
- **Venue / year:** NeurIPS 2023
- **Paper:** https://arxiv.org/abs/2310.18969
- **Tags:** `ViT` `mechanistic-interpretability` `class-embedding` `key-value-memory`

## Introduction
NLP 中可以把中间 hidden state 投影到 vocabulary space，观察语言模型如何逐层形成 token prediction；视觉 Transformer 没有同样常用的词表空间，因此其 image token 如何形成 class representation 仍不透明。本文把任意中间 image token 投影到 ViT 学到的 class embedding space，研究 class prototype 如何逐层形成，并比较 self-attention 与 MLP 的作用。

## Method / Framework
ViT 最终用 `[CLS]` 表征乘 class embedding matrix `E` 做分类。作者直接计算任意 block/token 的 `E x_i^b`，用正确类别在 logits 排名中的 identifiability 衡量该 image token 是否已携带 class evidence。参数分析把 MLP 的 `W_in/W_out` 与 self-attention 的 key/value 解释成 key-value memory：key 匹配输入模式，value 向 residual 写入 class prototype 更新。

框架还把每个 block 的 token class evidence 反投影为空间图，识别对目标类别最有贡献的 image patches；模型覆盖 ViT-B/16、B/32、L/16、CIFAR100、不同预训练和 ViT-GAP 变体。对照是传统 linear probing，后者能检测表征含信息，却不一定筛出模型实际用于分类的特征。

## Baselines / Comparisons
比较维度包括 random-initialized model、不同 patch/depth/dataset/architecture ViT、self-attention vs MLP、class token vs image token，以及 class-embedding projection vs linear probe。评价使用 class identifiability、class-value agreement、key-value agreement 和 patch-level explanation，而不是只看最终 accuracy。

## Experiments / Findings
image tokens 从早期 block 就逐渐与正确 class prototype 对齐，attention/context 会影响这种对齐；后层 image tokens 和最后 block 的 self-attention 为分类提供强信息。MLP 与 self-attention 都像 key-value memory，但 deeper blocks 的 class-value agreement 高于随机权重，且 MLP peak 通常更高，说明 MLP value 更强地推动 categorical updates。

时间位置上，早期 self-attention 的 key-value agreement 高于 MLP，MLP 在倒数第二层附近达到峰值，self-attention 在最后层达到峰值；attention 更新作用于更多 image tokens，MLP 更新更强但更局部。方法在类表示出现的早期阶段比 linear probing 更有效地揭示“哪些 token/patch 真的为该类别服务”。

## Ablation / Error Analysis
不同 ViT 变体大体复现 class-space 对齐，但 ViT-GAP 是明显例外：它平均 image token 而非只依赖 `[CLS]`，所以 categorical representation 可能分布在多个 value vectors，单一 token 的 key-value 图不完整。MLP/self-attention 分工还受 block 深度、patch size、上下文和 attention perturbation 影响。

linear probe 的高可预测性不等于模型使用了该特征，这是本文相对 baseline 的核心错误分析；class-space projection 也只围绕模型已学到的类别，不能解释未知类别或开放词汇语义。key 的输入语义仍未被完整解码，当前主要解释 value/class update。

## Limitations
实验集中在图像分类 ViT，不能直接推广到多模态 LLM 的语言对齐；class embedding 是有限类别的解释空间，且 projection/identifiability 仍是相关性指标。key-value memory 的解释依赖线性分解，复杂 attention 交互和 residual circuit 的因果验证留待后续。

## 两句话总结
本文为 ViT 构造 class-embedding lens，把中间 image token 和 MLP/attention value 投影成可读的类别证据，从而观察类别表征的逐层形成。实验显示 MLP 更新更强、attention 更新更广且更晚，但 ViT-GAP、linear-probe 伪解释和缺少 causal intervention 说明该框架是机制诊断工具而非完整因果证明。

## Evidence note
已逐段读取本地 PDF，核对了 class-space projection、identifiability/key-value 指标、ViT 变体、attention/MLP 峰值、patch explanation 与 ViT-GAP 异常。
