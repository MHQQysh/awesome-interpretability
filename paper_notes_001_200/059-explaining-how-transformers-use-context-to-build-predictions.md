# 059. Explaining How Transformers Use Context to Build Predictions

> 逐篇阅读记录：第 59 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Javier Ferrando, Gerard I. Gállego, Ioannis Tsiamas, Marta R. Costa‐jussà
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.acl-long.301)
- **领域标签**：Attribution, Rationale, Transformer, Layer, Explain
- **本地 PDF 文本规模**：约 13,754 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Language Generation Models produce words based on the previous context.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；X Neubig (2022) rules to find the evidence (in previ-；3 Experimental Setup nouns with an opposite number to that of the main；5 Results；8 Related Work the model in determining predictions that conform；11 Ethics statement vances in Neural Information Processing Systems,；2023. Inseq: An interpretability toolkit for sequence；V At,j
- **引言关键线索**：show the final logit contributions. The contrastive exten- Language Generation Models, like Transformer- sion of our proposed method, ALTI-Logit, shows that based Language Models (Brown et al., 2020; Zhang the model relies on the head of the subject (report) to correctly solve the subject-verb agreement. See expla- et al., 2022a) have recently revolutionized the field nations from other methods in Table 3. GPT-2 Small of Natural Language Processing (NLP). Despite shown here, see GPT-2 XL ALTI-Logit explanation in this, there is still a gap in our understanding of Appendix H.2. how they are able to produce language that closely resembles that of humans. This means that we are unable to determine the cause of a model鈥檚 (Jain and Wallace, 2019; Serrano and Smith, 2019; failure in specific instances, which can result in the Pruthi et al., 2020), and on applying gradient-based generation of h
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tions for a model鈥檚 prediction, it is still unclear L11 / A report about the Impressionists has how prior words affect the model鈥檚 decision L10 / A report about the Impressionists has throughout the layers. In this work, we lever- age recent advances in explainability of the L9 / A report about the Impressionists has Transformer and present a procedure to analyze L8 / A report about the Impressionists has models for language generation. Using con- L7 / A report about the Impressionists has trastive examples, we compare the alignment L6 / A report about the Impressionists has of our explanations with evidence of the lin- guistic phenomena, and show that our method L5 / A report about the Impressionists has consistently aligns better than gradient-based L4 / A report about the Impressionists has and perturbation-based baselines. Then, we L3 / A report about the Impressionists has investigate the role of MLPs inside the Trans- former and s
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Transformer, Layer, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Then, Simonyan et al, Table 3, Comparison of different contrastive, Note that for, We also ana-, Table 9, Table 10, Table 11
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - trastive examples, we compare the alignment
  - and perturbation-based baselines. Then, we
  - at a baseline point (Simonyan et al., 2014),
  - contrastive setting. In 搂5 we compare their expla-
  - Table 3: Comparison of different contrastive explanation 2 XL).
  - based and erasure-based baselines. Note that for

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：4 times, 5499
                 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - BLiMP the average we show in Figure 4 is across
  - 9 different subsets. In Table 3 we show an exam-
  - We find similar alignment results for Logit and
  - In the boxplots in Figure 6, we show the distribu- memories (Geva et al., 2021), where keys are stored
  - linguistic phenomena is higher. This conclusion ot = kil vil (17)
  - In Figure 8, we show some examples of the
  - movies. In dimension 3038 we find a value up- i j

## 6. Conclusion、局限与可复现性

- **结论段落线索**：holds for larger models too, see the darn exam- i ple in Appendix H.2, where large logit updates are where vil represents the i-th row of W2 . Recalling found in layers 28, 35, and 40, matching the layers how the final logit of a token w is decomposed where alignment peaks (Figure 7 Top). In IOI and by layer-wise updates in Equation (7), the MLPl SVA tasks both models align with the evidence and updates the logit of w as follows: increase their logit update towards the last layers. This indicates that models solve these phenomena 鈭唋ogitlw鈫怣LPl = olt Uw鈯 once they have acquired sufficient contextual infor- dmlp X mation. = kil vil Uw鈯 Our findings in the IOI task support those by i (18) Wang et al. (2023). In GPT-2 Small we observe dmlp X high logit difference updates coming from the In- = 鈭唋ogitlw鈫恔l vl i i direct Object (IO) in layers 10 and 11. We further i study the heads in those layers (Table 4), where Thus, the update of the MLP can be decomposed Wang et al. (2023) found 鈥楴ame Mo
- **局限/未来工作线索**：alignments. A limitation of using this method to et al. (2022) study how the different Transformer；10 Limitations you find these shortcuts?鈥 a protocol for evaluating；3 A1. Did you describe the limitations of your work?
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Explaining How Transformers Use Context to Build Predictions”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
