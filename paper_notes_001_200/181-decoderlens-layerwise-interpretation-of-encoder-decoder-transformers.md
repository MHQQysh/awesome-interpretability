# 181. DecoderLens: Layerwise Interpretation of Encoder-Decoder Transformers

> 逐篇阅读记录：第 181 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Anna Langedijk, Hosein Mohebbi, Gabriele Sarti, Willem Zuidema, Jaap Jumelet
- **发表 venue / date**：NAACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-naacl.296)
- **领域标签**：Representation, Lens, Reasoning, Hidden, Layer, Analyze
- **本地 PDF 文本规模**：约 11,907 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：In recent years, several interpretability methods have been proposed to interpret the inner workings of Transformer models at different levels of precision and complexity.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；3 DecoderLens 4 Factual Trivia QA；6 Machine Translation；0 Table 4: Example DecoderLens translations for an En-；7 Speech-to-Text ate a limited number of irrelevant words (notably,；8 Conclusion；2018 Conference on Empirical Methods in Natural；2023. Not all layers are equally as important: Every Association for Computational Linguistics: Human
- **引言关键线索**：only Transformers, and is unable to explain how Many methods for interpreting the internal states representations evolve in the encoder of encoder- of neural language models 鈥 and in particular decoder models. Transformer-based models 鈥 have been proposed in Concretely, DecoderLens forces the decoder the last few years (for a review, see Lyu et al., 2024). module of an encoder-decoder model to cross- Such methods operate at many different levels of attend intermediate encoder activations. As a conse- granularity, ranging from model-agnostic attribu- quence, its generations can be seen as sequences of tion methods that treat models as black-boxes, to vocabulary projections depending only on partially- probing methods that assess whether specific infor- formed source-side representations. Such adapta- mation is decodable from model representations, tion is necessary as LogitLens requires t
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：allows the decoder to cross-attend representa- What is the capital of Spain? tions of intermediate encoder activations in- stead of using the default final encoder out- Figure 1: Schematic overview of the DecoderLens. By put. The method thus maps uninterpretable using the decoder to cross-attend intermediate encoder intermediate vector representations to human- activation, we can gain qualitative insights into how interpretable sequences of words or symbols, representations evolve across encoder layers. shedding new light on the information flow in this popular but understudied class of models. of encoder-decoder Transformers as a 鈥渓ens鈥 to ex- We apply DecoderLens to question answering, plain the evolution of representations throughout logical reasoning, speech recognition and ma- model layers in these model architectures. Our chine translation models, finding that simpler method is directly inspired by the LogitLens (nos- subtasks are
- **方法与解释性关系**：该论文主要围绕 `Representation, Lens, Reasoning, Hidden, Layer, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Appendix C.1
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - same examples, providing an unbiased comparison reported in Appendix C.1.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - We find that intermediate outputs can be use- layer. As the method projects into the output (logit)
  - improve the final performance (Dou et al., 2018; In the following sections, we investigate the effec-
  - easier formulas can be solved in earlier layers. chance. Moreover, we find that initial layers of-
  - sees the largest improvement for all other types of
  - troducing the DecoderLens, we provide insights improvement through early exiting strategies.
  - and Mor Geva. 2023. Jump to conclusions: Short- local solutions in all cases.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：includes both source and translation references for Our work contributes to a growing body of research each utterance, we can inspect Whisper鈥檚 behavior on the interpretability of language models. By in- for both transcription and translation tasks on the 6 The same pattern is observed for the other model sizes, same examples, providing an unbiased comparison reported in Appendix C.1. between the tasks. 7 We report these results to Appendix C.2. 4771 troducing the DecoderLens, we provide insights improvement through early exiting strategies. into how intermediate encoder representations of encoder-decoder Transformers influence decoder 9 Limitations predictions. We apply our method to various mod- els and tasks, finding that intermediate outputs can One important concern regarding the direct use of provide valuable insights into the model鈥檚 decision- intermediate representations to make predictions making process. In particular, our findings reveal is that of representational drift: fe
- **局限/未来工作线索**：encoder-decoder Transformers influence decoder 9 Limitations；Future work could explore the application of De- sity of Amsterdam (JJ, WZ), and from the Nether-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“DecoderLens: Layerwise Interpretation of Encoder-Decoder Transformers”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
