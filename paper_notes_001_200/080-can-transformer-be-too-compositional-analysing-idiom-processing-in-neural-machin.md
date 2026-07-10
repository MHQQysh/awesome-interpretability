# 080. Can Transformer be Too Compositional? Analysing Idiom Processing in Neural Machine Translation

> 逐篇阅读记录：第 80 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Verna Dankers, Christopher J. Lucas, Ivan Titov
- **发表 venue / date**：ACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.acl-long.252)
- **领域标签**：Representation, Hidden, Behavior, Analyze
- **本地 PDF 文本规模**：约 13,264 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Unlike literal expressions, idioms' meanings do not directly follow from their parts, posing a challenge for neural machine translation (NMT).
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction nisms enabling idiomatic translations and methods；3 Method 1；4 Attention sentences with a literal PIE and a word-for-word；7 Conclusion by the UKRI Centre for Doctoral Training in Nat-；2009. The development of idiom comprehension:；2021. On compositional generalization of neural Guarding protected attributes by iterative nullspace；2019 Conference on Empirical Methods in Natu-
- **引言关键线索**：An idiom is a group of words of which the figura- for improving them, other than data annotation (Za- tive meaning differs from the literal reading, such ninello and Birch, 2020). Related work studies as 鈥渒ick the bucket,鈥 which means to die, instead how idioms are represented by Transformer-based of physically kicking a bucket. An idiom鈥檚 figu- language models (e.g. Garc铆a et al., 2021a,b), but rative meaning is established by convention and those models are not required to output a discrete is typically non-compositional 鈥 i.e. the meaning representation of the idiom鈥檚 meaning, which is a cannot be computed from the meanings of the id- complicating factor for NMT models. iom鈥檚 parts. Idioms are challenging for the task of In this work, we analyse idiom processing for neural machine translation (NMT) (Barreiro et al., pre-trained NMT Transformer models (Vaswani 2013; Isabelle et al., 20
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：An idiom is a group of words of which the figura- for improving them, other than data annotation (Za- tive meaning differs from the literal reading, such ninello and Birch, 2020). Related work studies as 鈥渒ick the bucket,鈥 which means to die, instead how idioms are represented by Transformer-based of physically kicking a bucket. An idiom鈥檚 figu- language models (e.g. Garc铆a et al., 2021a,b), but rative meaning is established by convention and those models are not required to output a discrete is typically non-compositional 鈥 i.e. the meaning representation of the idiom鈥檚 meaning, which is a cannot be computed from the meanings of the id- complicating factor for NMT models. iom鈥檚 parts. Idioms are challenging for the task of In this work, we analyse idiom processing for neural machine translation (NMT) (Barreiro et al., pre-trained NMT Transformer models (Vaswani 2013; Isabelle et al., 2017; Constant et al., 2017; et al., 2017) for seven
- **方法与解释性关系**：该论文主要围绕 `Representation, Hidden, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Colson, The comparison can help, Context to PIE, Language comparison, Target to, We now turn to, We compare, Languages comparison for PIE, PIE and non-PIE nouns, The languages comparison dis-, Languages comparison, Dutch seven target languages, We compared hidden states, Context-PIE, Language comparison ferred to, Target -
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - speech are ubiquitous in natural language (Colson, The comparison can help identify mechanics that
  - (c) Context to PIE (d) Language comparison (c) Target to </s> (d) Language comparison
  - We now turn to comparing how literal and figura- translation are indicated by 鈥榣it-wfw鈥. We compare
  - (c) Languages comparison for PIE nouns 1 2 3 4 5 6 1 2 3 4 5 6
  - PIE and non-PIE nouns. The languages comparison dis- 0.015 nl
  - W and V are then used to project new data points (e) Languages comparison

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：20 times, 80 x, 160 x, 320 x, 640 x, 1280 x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - word? We find (1) decreased interaction between the
  - frequently than their parts, their meanings need for methods that improve idiomatic translations.
  - To assert that the conclusions drawn in this sec- tive, paraphrased instances, and the mean weights
  - idiom as one word improves translations. It also
  - Spanish improve idiomatic translations, future work could
  - improve the grouping of idioms as single units by
  - 7 Conclusion by the UKRI Centre for Doctoral Training in Nat-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tion are not simply explained by shallow statistics of the literal instances translated word for word.7 of the data used, we recompute the results in Ap- In other words, figurative PIEs are grouped more pendix C for (1) a data subset excluding variations strongly than their literal counterparts. of PIEs鈥 standard surface forms, (2) a data subset Attention between PIEs and context To exam- that includes PIEs that appear in both figurative and ine the interaction between a PIE and its context, literal contexts, (3) a data subset that controls for we obtain the attention weights from tokens within the number of tokens within a PIE. Qualitatively, the PIE to nouns in the surrounding context of size these results lead to the same findings. 10 (Figure 3b).8 Similarly, the attention from the Attention within the PIE For the En-Nl Trans- surrounding context to PIE nouns is measured (Fig- former, Figure 3a visualises the distribution of ure 3c). There is reduced attention from PIEs to attention
- **局限/未来工作线索**：Spanish improve idiomatic translations, future work could
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Can Transformer be Too Compositional? Analysing Idiom Processing in Neural Machine Translation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
