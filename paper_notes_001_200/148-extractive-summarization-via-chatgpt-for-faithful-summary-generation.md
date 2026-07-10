# 148. Extractive Summarization via ChatGPT for Faithful Summary Generation

> 逐篇阅读记录：第 148 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Haopeng Zhang, Xiao Liu, Jiawei Zhang
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.214)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 5,787 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Extractive summarization is a crucial task in natural language processing that aims to condense long documents into shorter versions by directly extracting sentences.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related Work；4 Experiments and Analysis
- **引言关键线索**：Our experimental analysis demonstrates that Chat- Document summarization aims to compress text GPT exhibits inferior extractive summarization per- material while retaining its most salient infor- formance in terms of ROUGE scores compared mation. With the increasing amount of pub- to existing supervised systems, while achieving licly available text data, automatic summarization higher performance based on LLM-based evalua- approaches have become increasingly important. tion metrics. Additionally, we observe that using These approaches can be broadly classified into an extract-then-generate pipeline with ChatGPT two categories: abstractive and extractive sum- yields large performance improvements over ab- marization. While abstractive methods (Nallapati stractive baselines in terms of summary faithful- et al., 2016; Gupta and Gupta, 2019) have the ad- ness. vantage of producing flexible a
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：benchmark datasets. Our experimental analy- Another study by (Zhang et al., 2023d) conducted sis reveals that ChatGPT exhibits inferior ex- a comprehensive analysis of large language models tractive summarization performance in terms for news summarization and found that the gener- of ROUGE scores compared to existing super- ated summaries were comparable to those produced vised systems, while achieving higher perfor- by humans. However, existing research (Yang et al., mance based on LLM-based evaluation met- 2023; Luo et al., 2023) has only focused on abstrac- rics. In addition, we explore the effectiveness of in-context learning and chain-of-thought rea- tive summary approaches, and the performance of soning for enhancing its performance. Further- ChatGPT for extractive summarization remains an more, we find that applying an extract-then- open question. Moreover, the hallucination prob- generate pipeline with ChatGPT yields signifi- l
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：ORACLE, ROUGE scores in comparison, SOTA fine-tuned baselines are
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tive baselines in terms of summary faithful- the need to explore extractive summarization with
  - stractive baselines in terms of summary faithful-
  - baselines without hurting summary qualities. model M based on the probability of whether it
  - generate-only baseline, the extract-then-generate pipeline, and generation based on ORACLE, respectively.
  - lower ROUGE scores in comparison to previous
  - reference summaries of the dataset and the limit only baselines (less than 10 percent), highlighting

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significant interest in the NLP community due text summarization tasks has sparked significant
  - more, we find that applying an extract-then- open question. Moreover, the hallucination prob-
  - cant performance improvements over abstrac- abstractive summarization systems, highlighting
  - yields large performance improvements over ab-
  - could improve the generated summary faithfulness existing work formulates it as a sequence label-
  - the previous conclusion in (Goyal et al., 2022; as presented in Table 3.
  - ChatGPT-Ext outperforms ChatGPT-Abs in two ex- The results show large improvements in sum-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Zhang et al., 2023d). We also observe that ChatGPT-Ext outperforms ChatGPT-Abs in two ex- The results show large improvements in sum- tractive datasets CNN/DM and PubMed while per- mary factual consistency across all four datasets forming worse in the other two abstractive datasets. with the extract-then-generate framework. Notably, We argue the results are due to the bias within the the FactCC scores are extremely low for generate- reference summaries of the dataset and the limit only baselines (less than 10 percent), highlighting of ROUGE scores. Nonetheless, we notice that de- the hallucination problems of ChatGPT-based sum- spite being primarily designed for generation tasks, marization, where ChatGPT tends to make up new ChatGPT achieves impressive results in extractive content in the summary. Nevertheless, the extract- summarization, which requires comprehension of then-generate framework effectively alleviates the the documents. The decoder-only structure of Chat- hallucination 
- **局限/未来工作线索**：Limitations stette, Lasse Espeholt, Will Kay, Mustafa Suleyman,
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Extractive Summarization via ChatGPT for Faithful Summary Generation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
