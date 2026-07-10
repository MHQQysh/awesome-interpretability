# 078. Metaphors in Pre-Trained Language Models: Probing and Generalization Across Datasets and Languages

> 逐篇阅读记录：第 78 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ehsan Aghazadeh, Mohsen Fayyaz, Yadollah Yaghoobzadeh
- **发表 venue / date**：ACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.acl-long.144)
- **领域标签**：Probing, Representation, Evaluation, Hidden, LLM, Layer, Detect
- **本地 PDF 文本规模**：约 8,004 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Human languages are full of metaphorical expressions.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction must encode some information about metaphors；2 Related Work；4 Experimental Setup and Results the obtained code lengths in bits (see extra details；2021. Not all models localize linguistic knowl- dar Joshi, Danqi Chen, Omer Levy, Mike Lewis,；2016 Conference of the North American Chapter of Methods in Natural Language Processing (EMNLP),
- **引言关键线索**：due to their great performance in metaphor detec- Pre-trained language models (PLMs) (Peters et al., tion and other language processing tasks. Con- 2018; Devlin et al., 2019), are now used in almost firming that experimentally is a question that we all NLP applications, e.g., machine translation (Li address here. Specifically, we aim to know whether et al., 2021), question answering (Zhang et al., generalizable metaphorical knowledge is encoded 2020), dialogue systems (Ni et al., 2021), and sen- in PLM representations or not. The outline of our timent analysis (Minaee et al., 2020). They have work is presented in Figure 1. sometimes been referred to as 鈥渇oundation models鈥 We first do probing experiments to answer ques- (Bommasani et al., 2021) due to their significant tions such as: (i) with which accuracies and ex- impact on research and industry. tractablities do different PLMs encode 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：a cognitive phenomenon associating two different probing (Tenney et al., 2019b) and minimum de- concepts or domains. This phenomenon is built in scription length (Voita and Titov, 2020), and apply cognition and expressed in language. The creativ- them to four metaphor detection datasets, namely ity and problem solving (i.e., generalization to new LCC (Mohler et al., 2016), TroFi (Birke and Sarkar, ? Equal contribution. 2006), VUA pos, and VUA Verbs (Steen, 2010). 2037 Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics Volume 1: Long Papers, pages 2037 - 2050 May 22-27, 2022 c 2022 Association for Computational Linguistics To better estimate the generalization of Tsvetkov et al. (2014) present cross-lingual metaphorical knowledge in PLMs, we design two metaphor detection models using linguistic fea- setups in which testing comes from a different tures and word embeddings. Bilingual dictionaries distri
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, Evaluation, Hidden, LLM, Layer, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Noun, Verb, ALL, Baseline BERT RoBERTa ELECTRA, ELECTRA, Baseline is a randomly, BERT, The edge probing results, MDL is shown to, PLMs are common baselines, Russian shows the best, Table 4. The random
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - given as well. N: Noun, V: Verb, ALL: Noun, Verb, baseline is 50% in all cases) and have split the
  - Baseline BERT RoBERTa ELECTRA
  - ELECTRA. Baseline is a randomly initialized BERT. The edge probing results are the average of three
  - wise comparisons. MDL is shown to be more effec-
  - same architecture as PLMs are common baselines Russian shows the best out-of-distribution results
  - Table 4. The random baseline is acquired using a set the train size of each to the minimum of all, i.e.,

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - (Bommasani et al., 2021) due to their significant tions such as: (i) with which accuracies and ex-
  - 鈥 We show that metaphorical knowledge is en- tion richness of the representations is inspected by
  - edge in PLM is and how consistent the annotations bution, we find out that there are considerable num-
  - We observe that XLM-R significantly outper- with randomized and one with pre-trained BERT
  - the POS, they have the same distribution. VUA spectively. To base our results, we include the
  - leverage better when testing and training sets are 5 Discussion and Conclusion
  - As additional transferability analysis, we com- lingual and cross-dataset experiments. Our results

## 6. Conclusion、局限与可复现性

- **结论段落线索**：from the same distribution. When the sets have Metaphors are important in human cognition, and different distributions, the biases do not transfer if we seek to build cognitively inspired or plausi- well. ble language understanding systems, we need to 4.3.3 Comparing cross-dataset and work more on their best integration in the future. cross-lingual Therefore, any work in this regard is impactful. Our probing experiments showed that PLMs do in fact represent the information necessary to do LCC(en) LCC(es) LCC(fa) LCC(ru) the task of metaphor detection. We assume this 82.31 78.02 77.3 78.04 information is related to metaphorical knowledge TroFi VUA POS VUA Verbs learned during pre-training. Further, the layer-wise analysis confirmed our hypothesis that middle lay- 60.54 68.61 67.15 ers are more informative. Table 6: Comparing cross-dataset and cross-lingual Even though our probing experiments did show scenarios using the same model (XLM-R), train- that metaphorical knowledge is present i
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Metaphors in Pre-Trained Language Models: Probing and Generalization Across Datasets and Languages”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
