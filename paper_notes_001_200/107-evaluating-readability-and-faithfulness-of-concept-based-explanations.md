# 107. Evaluating Readability and Faithfulness of Concept-based Explanations

> 逐篇阅读记录：第 107 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Meng Li, Haoran Jin, Ruixuan Huang, Zhihao Xu, Defu Lian, Zijia Lin, Di Zhang, Xiting Wang
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.36)
- **领域标签**：Representation, Rationale, Evaluation, Hidden, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 13,875 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：With the growing popularity of general-purpose Large Language Models (LLMs), comes a need for more global explanations of model behaviors.Concept-based explanations arise as a promising avenue for explaining high-level p
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Concept Evaluation Measures v T h, the above problems have closed-form solu-；6 Conclusion cally, building a more comprehensive landscape of；2019. Explainable recommendation through attentive terpretability beyond feature attribution: Quantitative；2024. Distillation with explanations from large lan-
- **引言关键线索**：hand (Koh et al., 2020), lacking a unified land- Explainable Artificial Intelligence (XAI) holds scape (C1). Moreover, its non-local nature across significant value in pre-trained language models鈥 samples (Kim et al., 2018), combined with the high mechanism understanding (Li et al., 2022), visual- cost of human evaluation when the number of con- ization (Yang et al., 2024), performance enhance- cepts is large, makes evaluating a concept鈥檚 read- ment (Wu et al., 2023b; Ribeiro et al., 2016; Wang ability challenging (C2). For available evaluation et al., 2022), and security (Burger et al., 2023; measures (Hoffman et al., 2018), it is difficult to * These authors contributed equally to this work. 鈥 test their reliability and validity (C3). Corresponding author: xitingwang@ruc.edu.cn 1 Codes available at https://github.com/hr-jin/ In this paper, we address the challenges above Concept-Explan
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：non-local nature and high dimensional rep- dararajan et al., 2017; Guan et al., 2019). The resentation in a model鈥檚 hidden space. Cur- attribution methods identify 鈥渨here鈥 the model rent methods approach concepts from differ- looks rather than 鈥渨hat鈥 it comprehends (Colin ent perspectives, lacking a unified formaliza- et al., 2022), typically offering local explanations tion. This makes evaluating the core measures of concepts, namely faithfulness or readabil- for a limited number of input samples, restrict- ity, challenging. To bridge the gap, we intro- ing their utility in practical settings (Colin et al., duce a formal definition of concepts generaliz- 2022; Adebayo et al., 2018). Concept-based ex- ing to diverse concept-based explanations鈥 set- planations (Kim et al., 2018; Cunningham et al., tings. Based on this, we quantify the faithful- 2023; Fel et al., 2023b) can mitigate the limita- ness of a concept explanation via perturbati
- **方法与解释性关系**：该论文主要围绕 `Representation, Rationale, Evaluation, Hidden, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Readability assesses the extent, Table 2, Comparison of readability measures, Sample, Comparison of Evaluation Measures, Compared with Our findings, We recommend EmbCos as, Comparison of Explanation Methods, Yet human evalu- different, Figure 4, Performance of different baselines, Table 5, Patterns that maximally activate, For
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - vary, making it difficult to ensure a fair comparison. truth label, y j , y 鈥瞛 are the logits of class j. To
  - Readability assesses the extent to which humans Table 2: Comparison of readability measures. #Sample
  - 5.2 Comparison of Evaluation Measures capture semantic similarity is also less desirable
  - ensure a fair comparison, we randomly sampled above are scored by each human labeler with a
  - proximation of human evaluation. Compared with Our findings are consistent across different lay-
  - computational cost. We recommend EmbCos as an 5.3 Comparison of Explanation Methods

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X, 8 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significant value in pre-trained language models鈥
  - 蟿 (Kendall, 1938) to measure pairwise correlation significantly lower correlation than measures of the
  - 6 Conclusion cally, building a more comprehensive landscape of
  - yield more insightful conclusions in the future. els across training and scaling. In International
  - sentative samples contribute more significantly to
  - in (Bills et al., 2023). We show a detailed
  - are the instructions for scoring each concept: will significantly contribute to the comprehensive-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：readability. This paper introduced two automatic evaluation Due to limited GPU resources and budget con- measures, readability and faithfulness, for concept- straints, we used smaller versions of LLM, focusing based explanations. We first formalize a general primarily on the 3rd layer of Pythia-70M for our definition of concepts and quantify faithfulness un- analysis. And our evaluation of the LLM-Score 615 was restricted to 200 concepts, incurring a cost of utterances for safety-alignment. arXiv preprint around $1 for a single concept. While this setup, arXiv:2308.09662. on par with (Cunningham et al., 2023) and more Stella Biderman, Hailey Schoelkopf, Quentin Gregory general than (Bricken et al., 2023), allowed for Anthony, Herbie Bradley, Kyle O鈥橞rien, Eric Hal- fast analysis and comparison with existing litera- lahan, Mohammad Aflah Khan, Shivanshu Purohit, ture, expanding our analysis to larger models could USVSN Sai Prashanth, Edward Raff, et al. 2023. Pythia: A suite for analyzi
- **局限/未来工作线索**：Coherence-based. To address these limitations, note readability on the input/output side using the；concept set varies widely in readability measures. Limitations；cess involves running the model to be interpreted new limitations in calculating logprobs on the in-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Evaluating Readability and Faithfulness of Concept-based Explanations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
