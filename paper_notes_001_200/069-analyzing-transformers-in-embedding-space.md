# 069. Analyzing Transformers in Embedding Space

> 逐篇阅读记录：第 69 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Guy Dar, Mor Geva, Ankit Gupta, Jonathan Berant
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.acl-long.893)
- **领域标签**：Probing, Representation, Transformer, Layer, Analyze
- **本地 PDF 文本规模**：约 20,609 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Understanding Transformer-based models has attracted significant attention, as they lie at the heart of recent technological advances across machine learning.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：Introduction / Method / Experiments / Conclusion
- **引言关键线索**：we can interpret such products as interactions between Transformer-based models [Vaswani et al., 2017] cur- pairs of vocabulary items. This applies to (a) interac- rently dominate Natural Language Processing [Devlin tions between attention queries and keys as well as to et al., 2018; Radford et al., 2019; Zhang et al., 2022] as (b) interactions between attention value vectors and the well as many other fields of machine learning [Doso- parameters that project them at the output of the atten- vitskiy et al., 2020; Chen et al., 2020; Baevski et al., tion module. Taking this perspective to the extreme, 2020]. Consequently, understanding their inner work- one can view Transformers as operating implicitly in ings has been a topic of great interest. Typically, work the embedding space. This entails the existence of a on interpreting Transformers relies on feeding inputs single linear space tha
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：puts, recent work has shown that an input- ing them by the embedding matrix to obtain a repre- independent approach, where parameters are sentation over vocabulary items.1 [Elhage et al., 2021] interpreted directly without a forward/back- have shown that in a 2-layer attention network, weight ward pass is feasible for some Transformer pa- matrices can be interpreted in the embedding space as rameters, and for two-layer attention networks. well. Unfortunately, their innovative technique could In this work, we present a conceptual frame- not be extended any further. work where all parameters of a trained Trans- In this work, we extend and unify the theory and former are interpreted by projecting them into findings of [Elhage et al., 2021] and [Geva et al., the embedding space, that is, the space of 2022b]. We present a zero-pass, input-independent vocabulary items they operate on. Focusing framework to understand the behavior of Transform
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, Transformer, Layer, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：As a baseline, MultiBERT seeds and compute, Rk of the activated, We bin S, More globally
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - score averaged across tokens per layer. As a baseline, parameters from two MultiBERT seeds and compute
  - we compare Rk of the activated vectors {x虃i }m i=1 of the the correlation between their projections to embedding
  - experiment, where we compare activated parameters from the two models. We bin S虄 by layer pairs and aver-
  - 0.70 racy that is significantly higher than the baseline 50%
  - and 0.83 for E T , showing the E T is better under when using top-k. More globally, we compare E 鈥 鈭 {E + , E T }

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：11 times, 0028646X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - attracted significant attention, as they lie at the et al., 2022b] showed that the value vectors of the
  - els. We show that in 30% of the cases, this procedure,
  - Center), we show that given two distinct instances of The Transformer consists of a stack of layers, each
  - we show how to interpret all the components of the
  - ding space. We show this is indeed empirically the case
  - E 鈥 to interact well with top-k. We show that the top cense: Apache-2.0).
  - medium. We show random pairs (k, v) from the set of

## 6. Conclusion、局限与可复现性

- **结论段落线索**：score averaged across tokens per layer. As a baseline, parameters from two MultiBERT seeds and compute we compare Rk of the activated vectors {x虃i }m i=1 of the the correlation between their projections to embedding correctly-aligned hidden state h虃 at the output of the rel- space. For example, let VA , VB be the FF values of evant layer (blue bars) against the Rk when randomly models A and B. We can project the values into em- sampling h虃rand from all the hidden states (orange bars). bedding space: VA EA , VB EB , where EA , EB are the We conclude that representations in embedding space respective embedding matrices, and compute Pearson induced by activated parameter vector mirror, at least correlation between projected values. This produces a to some extent, the representations of the hidden states similarity matrix S虄 鈭 R/VA /脳/VB / , where each entry themselves. Appendix 搂B.2 shows a variant of this is the correlation coefficient between projected values experiment, where we compar
- **局限/未来工作线索**：the non-linearities in the LM head for future work. former [Vaswani et al., 2017] relevant to our analysis.；While our work has limitations (see 搂8), we think the；benefits of our work overshadow its limitations. We；8 Limitations 2104.08696.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Analyzing Transformers in Embedding Space”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
