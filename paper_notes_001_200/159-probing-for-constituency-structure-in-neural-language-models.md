# 159. Probing for Constituency Structure in Neural Language Models

> 逐篇阅读记录：第 159 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：David Arps, Younes Samih, Laura Kallmeyer, Hassan Sajjad
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.findings-emnlp.502)
- **领域标签**：Probing, Representation, Hidden, LLM, Activation, Analyze
- **本地 PDF 文本规模**：约 10,594 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：In this paper, we investigate to which extent contextual neural language models (LMs) implicitly learn syntactic structure.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；1. Record the dependency context of each token Selectivity To evaluate if the the probe makes lin-；2. Replace a fraction of tokens with other tokens The higher the selectivity, the more one can be sure；6 Results for LCA prediction and
- **引言关键线索**：for the following reasons. In contrast to depen- Over the last years, neural language models (LMs), dency structure, it can be richer concerning the such as BERT (Devlin et al., 2019), XLNet (Yang represented abstract syntactic information, since it et al., 2019), RoBERTa (Liu et al., 2019b), and directly assigns categories to groups of tokens. On DistilBERT (Sanh et al., 2020), have delivered un- the other hand, not all dependency labels are rep- matched results in multiple key Natural Language resented in a standard constituency structure; but Processing (NLP) benchmarks (Wang et al., 2018). they can be incorporated as extensions of the cor- Despite the impressive performance, the black-box responding non-terminal nodes (see, e.g., the PTB nature of these models makes it difficult to ascer- label NP-SBJ in App. A.1). To quantify the gain tain whether they implicitly learn to encode lin
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：that mean pool results in lossy representation, and 6, and 7 discuss our experiments and their results, we recommend concatenation of representations as and Sec. 8 concludes. a better way to probe for relations between words. A difficulty when probing a LM for whether cer- 2 Related work tain categories are learned is that we cannot be sure that the LM does not learn a different category in- Syntactic information in neural LMs An im- stead that is also predictive for the category we portant recent line of research (Adi et al., 2017; are interested in. More concretely, when probing Hupkes et al., 2018; Zhang and Bowman, 2018; for syntax, one should make sure that it is not se- Blevins et al., 2018; Hewitt and Manning, 2019; mantics that one finds and considers to be syntax Hewitt and Liang, 2019; Reif et al., 2019; Tenney (since semantic relations influence syntactic struc- et al., 2019a; Manning et al., 2020; Hall Maudslay ture). This p
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, Hidden, LLM, Activation, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Syntactic and semantic knowledge, Baselines, To ensure that the, We use three baselines, PTB data as well, Random BERT The first, PTB for this baseline, This baseline is used, See Table 1 for, Individual tokens This baseline, LCA
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tic structures, such as constituency or dependency dependency trees, we compare the unlabeled brack-
  - non-manipulated dataset in comparison to 51.4% vised a set of edge probing tasks to get new insights
  - with a random representation baseline. on what is encoded by contextualized word embed-
  - 5.1.1 Syntactic and semantic knowledge 5.4 Baselines
  - To ensure that the probing classifier captures syn- We use three baselines to put the results of our
  - use the original PTB data as well as two modified Random BERT The first baseline is used in all

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：31 points, 2.2 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - version of the PTB. We find that 4 pretrained
  - learned by the LM. Moreover, we show that A number of studies have probed LMs for de-
  - guistic structure are learned by LMs (Tenney et al., of a subtree. We find that while 97% of the brack-
  - the constituency tree are present in the dependency we find that different sets of neurons capture
  - than dependency trees. A further reason for focus- 鈥 We show that a simple linear probe is effec-
  - of structure most linguistic theories use. tic properties and we show that a full con-
  - 鈥 We find that constituency structure is linearly

## 6. Conclusion、局限与可复现性

- **结论段落线索**：put dimensionality of the classifier is restricted, but the probe can use information from different parts Our experiments have shown that different pre- of the LM. The probe is trained (evaluated) on all trained LMs encode fine-grained linguistic infor- 38k (5.5k) sentences of the training (development) mation that is also present in constituency trees. split of the PTB. We find that the constituency trees More specifically, LMs are able to identify proper- reconstructed from different LMs are of high qual- ties of different types of constituents, such as S, NP, ity (Tab. 3, App. A.9). We achieve a labeled F1 VP, PP. Good results on the chunking task show score of 82.6 on the non-manipulated dataset for that the classifiers are able to combine knowledge RoBERTa (80.5 for XLNet, 80.4 for BERT) which about the kind of constituents that a token is part is 31 points better than the random BERT base- of, and knowledge of the position of a token in line. This outperforms the result from Vil
- **局限/未来工作线索**：In future work, we plan to extend this syntac-；Limitations representational power of neural machine translation；For a general discussion of the limitations of su- Durrani, Jia Xu, and Hassan Sajjad. 2022. Discover-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Probing for Constituency Structure in Neural Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
