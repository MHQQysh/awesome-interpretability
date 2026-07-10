# 144. Large Language Models Can Self-Improve

> 逐篇阅读记录：第 144 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jiaxin Huang, Shixiang Gu, Le Hou, Yuexin Wu, Xuezhi Wang, Hongkun Yu, Jiawei Han
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.67)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 12,082 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Large Language Models (LLMs) have achieved excellent performances in various tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction In this paper, we study how an LLM capa-；3 Method；5 Experiments and Results；4 Experimental Setup We conduct a series of experiments to demonstrate；60 LMSI；81 DROP combine large-scale generated data from LMSI；80 GSM8K and existing supervised data, to further improve the；6 Conclusions prohibitively high for most researchers to conduct
- **引言关键线索**：ble of in-context few-shot learning and chain-of- Scaling has enabled Large Language Models thought reasoning, is able to self-improve its rea- (LLMs) to achieve state-of-the-art performance on soning ability without supervised data. We show a range of Natural Language Processing (NLP) that using only input sequences (without ground tasks (Wang et al., 2018, 2019; Rajpurkar et al., truth output sequences) from multiple NLP task 2016). More importantly, new capabilities have datasets, a pre-trained LLM is able to improve per- emerged from LLMs as they are scaled to hun- formances for both in-domain and out-of-domain dreds of billions of parameters (Wei et al., 2022b): tasks. Our method is shown in Figure 1: we in-context few-shot learning (Brown et al., 2020) first sample multiple predictions using few-shot makes it possible for an LLM to perform well Chain-of-Thought (CoT) (Wei et al., 2
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：nificantly improves the general reasoning abil- instructions; Minerva (Lewkowycz et al., 2022) ity of PaLM 540B model (74.4%鈫82.1% on parsed the full ArXiv database carefully for rele- GSM8K, 90.0%鈫94.4% on OpenBookQA, and 63.4%鈫67.9% on ANLI-A3) and can vant articles to excel on challenging competitive also be adapted to extreme low-resource cases math and science datasets. The need for large anno- where even training questions and CoT prompts tated data for supervised LLM training still remains are limited. We conduct ablation studies and a burden for low-resource applications or specific show that fine-tuning on diverse reasoning domains where only limited annotations are avail- paths is critical for self-improvement. able.
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：In this work, Table 4, Comparison of CoT-prompting accuracy, Out-Of-Domain benchmarks with or, Kojima et al, CoT-prompting in Table 7, It is interest-
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - without external inputs. In this work, we the model performances beyond few-shot baselines
  - Table 4: Comparison of CoT-prompting accuracy results on six Out-Of-Domain benchmarks with or without training
  - questions still improves the reasoning ability of baseline performance of Kojima et al. (2022) plus self-
  - 2022) baseline (66.2% vs 53.8% at 10 paths, 74.2% results of CoT-prompting in Table 7. It is interest-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：74.4%, 90.0%, 63.4%, 45 points, 10 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Large Language Models Can Self-Improve
  - lutions as target outputs. We show that with- many human answers for diverse sets of text in-
  - nificantly improves the general reasoning abil- instructions; Minerva (Lewkowycz et al., 2022)
  - paths is critical for self-improvement. able.
  - Scaling has enabled Large Language Models thought reasoning, is able to self-improve its rea-
  - (LLMs) to achieve state-of-the-art performance on soning ability without supervised data. We show
  - 2016). More importantly, new capabilities have datasets, a pre-trained LLM is able to improve per-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：We demonstrated that a Large Language Model empirical studies along this line, we believe that (LLM) is capable of improving its performance on our findings are conceptually useful for the NLP reasoning datasets by training on its own generated community by providing new insights for the prop- labels, given input questions only. Experiments erties of large language models. using the PaLM model with 540 billion parameters Acknowledgments show that LMSI improves the accuracy scores by 1.1% to 7.7% on six datasets, without training on We thank anonymous reviewers for valuable and ground truth labels. Furthermore, we show that insightful feedback. 1059 References Hyung Won Chung, Le Hou, Shayne Longpre, Barret Zoph, Yi Tay, William Fedus, Eric Li, Xuezhi Wang, Massih-Reza Amini, Vasilii Feofanov, Loic Pauletto, Mostafa Dehghani, Siddhartha Brahma, Adams Emilie Devijver, and Yury Maximov. 2022. Self- Yu, Albert Webson, Xinyun Chen, Gaurav Mishra, training: A survey. Zhuyun Dai, Shixiang Sha
- **局限/未来工作线索**：ical results could inspire more future work by the learning setting, where we do not assume we have；prompts. As part of our future work, we plan to
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Large Language Models Can Self-Improve”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
