# 157. Do Transformers Parse while Predicting the Masked Word?

> 逐篇阅读记录：第 157 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Haoyu Zhao, Abhishek Panigrahi, Rong Ge, Sanjeev Arora
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.1029)
- **领域标签**：Probing, Representation, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 22,909 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Pre-trained language models have been shown to encode linguistic structures like parse trees in their embeddings while being trained unsupervised.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction Cotterell (2021) suggested that linear probing；20 NN, Layer 0 mantic relations do not appear in the pre-trained；2019. Visualizing and measuring the geometry of Chulhee Yun, Yin-Wen Chang, Srinadh Bhojanapalli,；50 A12L12 50；45 A12L6 45 A3L12；40 A12L3 40 A12L12；35 A12L1 35 A24L12；55 Sentence F1 55
- **引言关键线索**：One of the surprising discoveries about transformer- relies on semantic cues for parsing. They created based language models like BERT (Devlin et al., syntactically correct but semantically meaningless 2019) and RoBERTa (Liu et al., 2019) was that sentences and found a significant drop in parsing contextual word embeddings encode information performance compared to previous studies. about parsing, which can be extracted using a (Qs 2): Do BERT-like models trained for simple 鈥渓inear probing鈥 to yield approximately masked language modeling (MLM) encode correct dependency parse trees for the text (Hewitt syntax, and if so, how and why? and Manning, 2019; Manning et al., 2020). Sub- sequently, Vilares et al. (2020); Wu et al. (2020); 1.1 This paper Arps et al. (2022) employed linear probing also to To address Qs 1, we construct a transformer that ex- recover information about constituency pa
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：December 6-10, 2023 漏2023 Association for Computational Linguistics length of the sentence is L, our constructed trans- in particular a process related to the Inside-Outside former has 2L layers in total, N attention heads, algorithm, to achieve low MLM loss. and 2N L embedding dimensions in each layer. However, this is massive compared to BERT. For 2 Preliminaries PCFG learned on Penn Treebank (PTB) (Marcus 2.1 Attention et al., 1993), N = 1600, average L 鈮 25, which leads to a transformer with 80k embedding dimen- We focus on encoder-only transformers like BERT sion, depth 50, and 1.6k attention heads per layer. and RoBERTa (Devlin et al., 2019; Liu et al., 2019), By contrast, BERT has 768 embedding dimensions, which stack identical layers with an attention 12 layers, and 12 attention heads per layer! module followed by a feed-forward module. Each attention module has multiple heads, represented One potential explanation could be that
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Comparison Between Different Probes, PCFG, PTB since se-, Figure 1, Comparison between different probes, AiLj, PCFG and PTB, We find that except, PTB, A3L12, Right-branching, Chen et al, Between Different Settings, Figure 2, Sam McCandlish, Chris Olah. 2022. In-context
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Comparison Between Different Probes probing: train and test the probe on synthetic PCFG
  - 30 as a baseline for a syntactic probe on PTB since se-
  - Figure 1: Comparison between different probes (linear
  - denote the models with AiLj, where i and j indi- about 5% compared with PCFG and PTB, but they
  - for various models. We find that except for models and thus gives a baseline for the probes on PTB
  - heads (A3L12), other models have nearly the same a comparison, the naive baseline, Right-branching

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：200
 points, 1      X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - is of significant interest, as incorporating (the
  - of generative modeling with PCFGs. We show et al., 2020b; P茅rez et al., 2021) established that
  - 2019) and RoBERTa (Liu et al., 2019) was that sentences and found a significant drop in parsing
  - for various models. We find that except for models and thus gives a baseline for the probes on PTB
  - heads does not appear to improve the result. F1 score (Li et al., 2020) on PTB dataset, and if we
  - probability of span [i, j] and 碌(A, i, j) is given by three different settings. Surprisingly, we find
  - pre-trained models. The results show that the neu- (Appendix A.4). We sketch the control task design

## 6. Conclusion、局限与可复现性

- **结论段落线索**：鈥渟ensitivity metric鈥 (see Figure 1, where the sensi- works have investigated specific classes of lan- tivity of the linear probe is greater than that of the guages, e.g. bounded-depth Dyck languages (Yao 2-layer NN probe). Experiment results for marginal et al., 2021), modular prefix sums (Anil et al., probability control task (Appendix A.4) also sup- 2022), adders (Nanda et al., 2023), regular port the alignment of Selectivity and Sensitivity. languages (Bhattamishra et al., 2020a), and sparse logical predicates (Edelman et al., 2022). Merrill
- **局限/未来工作线索**：2020a. On the ability and limitations of transform-；Limitation ceedings of the 24th Conference on Computational；We believe that the main limitations of our study；Due to limitations imposed by GPU resources, Chuanqi Tan, Mosha Chen, and Liping Jing. 2021.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Do Transformers Parse while Predicting the Masked Word?”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
