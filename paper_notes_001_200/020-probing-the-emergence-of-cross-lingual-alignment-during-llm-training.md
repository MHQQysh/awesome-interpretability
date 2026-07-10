# 020. Probing the Emergence of Cross-lingual Alignment during LLM Training

> 逐篇阅读记录：第 20 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Hetong Wang, Pasquale Minervini, Edoardo Maria Ponti
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.724)
- **领域标签**：Probing, Representation, Hidden, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 11,270 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Multilingual Large Language Models (LLMs) achieve remarkable levels of zero-shot crosslingual transfer performance.We speculate that this is predicated on their ability to align languages without explicit supervision fro
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction Tense for verbs) tend to activate the same subnet-；3 Experimental Setup they are split into train, validation, and test sets so；4 Results these LMs usually display a steady acquisition of；2020. On the cross-lingual transferability of mono- hal Nayak, Ryan Teehan, Samuel Albanie, Sheng；2020. XTREME: A massively multilingual multi-
- **引言关键线索**：work within LMs. This implies that the more two Language Models (LMs) pre-trained on unlabelled languages are aligned, the higher the overlap of the multilingual texts show remarkable performance in subsets of neurons encoding their information. zero-shot cross-lingual transfer (BigScience Work- We speculate that the degree of alignment tends shop et al., 2023; Xue et al., 2021; Conneau et al., to increase during pre-training, and that this fa- 2020a). In fact, fine-tuning a multilingual LM on cilitates the emergence of zero-shot transfer capa- annotated data for a downstream task in a source bilities. To corroborate this hypothesis, we cal- language allows it to perform inference in other culate the correlation between intrinsic metrics of target languages, too鈥攁lthough often with vary- alignment and cross-lingual downstream task per- ing degrees of degradation (Pires et al., 2019; Wu f
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：ing neurons 1 to k. grouped by linguistic feature (part of speech, num- ber, gender, etc.) and randomly shuffled. Finally,
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, Hidden, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Pairwise Overlap Comparison
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - B.1 Pairwise Overlap Comparison

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：20 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - find a statistically significant, strong correlation = p胃 (蟺 / h, C) p(C),
  - 2022), where a significant overlap is observed at training set of 392k samples and 10 epochs on POS
  - The level of cross-lingual alignment during pre- steep increase at the beginning. In contrast, we find
  - however, they differ significantly across scales. As BLOOM560m , BLOOM1b1 , and BLOOM1b7
  - crease of neuron overlap. First, we find that the a monotonic growth in neuron overlap. Thus, the
  - and significant lends credibility to our claim that cross-lingual alignment are in agreement with the
  - into bad minima depends on their scale. Xia et al. examples or anchor points, which improves zero-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：our work contributes to understanding how mul- In this paper, we probe a collection of checkpoint tilingual LMs implicitly align information across models of BLOOM to study the dynamics of mul- languages even in the absence of parallel data. tilingual pretraining. By experimenting with three model sizes, we observe that the subset of neu- Limitations rons encoding linguistic features tends to increase their overlap across languages throughout pretrain- Our work focuses on the checkpoints of BLOOM ing. Nevertheless, we also detect severe drops that with large intervals in global steps. Thus, our find- occur at different points in the training process, es- ings on the trend of alignment might be not applica- pecially at smaller model scales, instead of a steady ble if zooming in on a particular window of training increase in the extent of alignment. with finer-grained checkpoint models. Moreover, Moreover, we corroborate the hypothesis that the we consider only autoregressive models with
- **局限/未来工作线索**：model sizes, we observe that the subset of neu- Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Probing the Emergence of Cross-lingual Alignment during LLM Training”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
