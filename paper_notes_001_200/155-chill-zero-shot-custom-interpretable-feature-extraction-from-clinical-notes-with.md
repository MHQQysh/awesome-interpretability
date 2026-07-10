# 155. CHiLL: Zero-shot Custom Interpretable Feature Extraction from Clinical Notes with Large Language Models

> 逐篇阅读记录：第 155 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Denis McInerney, Geoffrey S. Young, Jan-Willem van de Meent, Byron Wallace
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.568)
- **领域标签**：Probing, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 10,118 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Methods nary features will be imperfect, and are naturally；3 Evaluation；4 Results spond to ill-defined precision (no positive predictions)；5 Discussion that this approach yields performance comparable
- **引言关键线索**：LLMs have greatly advanced few- and zero-shot ca- that LLMs are capable zero-shot extractors of clin- pabilities in NLP, reducing the need for annotation. ical information, i.e., they can extract structured This is especially exciting for the medical domain, information from records without explicitly being in which supervision is often scant and expensive. trained to do so. These can be high-level features However, given the high-stakes nature of clinical specified in natural language, which allows prac- work and the challenges associated with developing titioners to define features that they hypothesize models (e.g., long-tail data distributions, weakly may be relevant to a downstream task. Such fea- informative supervision), predictions can rarely be tures, even if noisy, are likely to be more inter- trusted blindly. Clinicians therefore tend to favor pretable than low-level features 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：[0 0 鈥 1 鈥 0 1] x x fication of features for linear models. CHiLL BP 133/81 / Wt 190 lbs chronic <latexit sha1_base64="QXWNndi5NRieIutz8eBLFzeTjJc=">AAAB6XicbVBNS8NAEJ34WetX1aOXxSJ4KkkV9Fj04rGK/YA2lM120y7dbMLuRCyl/8CLB0W8+o+8+W/ctDlo64OBx3szzMwLEikMuu63s7K6tr6xWdgqbu/s7u2XDg6bJk414w0Wy1i3A2q4FIo3UKDk7URzGgWSt4LRTea3Hrk2IlYPOE64H9GBEqFgFK10/1TslcpuxZ2BLBMvJ2XIUe+Vvrr9mKURV8gkNabjuQn6E6pRMMmnxW5qeELZiA54x1JFI278yezSKTm1Sp+EsbalkMzU3xMTGhkzjgLbGVEcmkUvE//zOimGV/5EqCRFrth8UZhKgjHJ3iZ9oTlDObaEMi3srYQNqaYMbThZCN7iy8ukWa1455Xq3UW5dp3HUYBjOIEz8OASanALdWgAgxCe4RXenJHz4rw7H/PWFSefOYI/cD5/ABwajRQ=</latexit> 鈥 prompts LLMs with expert-crafted queries Referal(s): 鈥 illness indicator to generate interpretable features from health Yes records. The resulting noisy labels are then Raw patient notes Prompt LLM Linear model over high- used to train a simple linear classifier. Gener- (Flan-T5) level features ating features based on queries to an LLM can em
- **方法与解释性关系**：该论文主要围绕 `Probing, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：And finally, TF-IDF, Here we compare the, Specifically, Goodman, Livio Baldini Soares, Haitang
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - 0.6 tasks. And finally, we compare against direct zero-
  - ring arbitrary clinically salient high-level features range of 10-105 as compared with 30k for TF-IDF
  - Here we compare the top-ranked features in linear not spend time searching for, or reporting the asso-
  - orax, a cause of passive atelectasis, and so ratio- to baselines in terms of data efficiency. Specifically,
  - dataset with uncertainty labels and expert comparison. bastian Goodman, Livio Baldini Soares, Haitang

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - evaluate this approach. We find that linear mod-
  - this work we show that despite the specialized do-
  - ploit LLM calibration to improve performance, al-
  - We show that the resulting linear models鈥 weights
  - For the three standard clinical prediction tasks we our results鈥擟onsolidation and Pleural Effusion鈥
  - improve AUROC for the three tasks considered. it unlikely that this would impact performance much.
  - and an 鈩2 penalty). We show the full prompt tem- Chest X-ray data. Histograms are over features. For

## 6. Conclusion、局限与可复现性

- **结论段落线索**：to contextualize the predictive performance of the pretability of zero-shot extracted features.) Finally, models evaluated we also consider several base- we also find that using binary features instead of lines that offer varying degrees of 鈥渋nterpretability鈥. continuous features degrades performance signifi- Specifically, we evaluate fine-tuned variants of (a) cantly (Table 1); calibrating (un)certainty helps. Clinical BERT (Alsentzer et al., 2019), and (b) a When manually inspecting some example radi- logistic regression model defined over BoW (TF- ology reports, it became apparent that downstream labels are often verbalized in the radiology report. w/ Custom Pheno. Readmis. Mort. CXR Class. 5 Expanded set of results in Apppendix Table C.1. 鉁 -.054 -.025 -.052 - 6 鉁 -.050 -.023 -.088 -.060 Inferred codes are not expected to fully align with noisy ICD codes (see section 4.1). 7 Performance is not 100% because only a subset of the Table 1: Difference in AUROC between using Binary ICD c
- **局限/未来工作线索**：to calculate precision, recall, and F1 for binary this may be partially due to a limitation of the ICD；issues discussed in the Limitations section, below. verifiable features does not guarantee that clini-；Limitations Can we manually intervene when a model learns；ifiable. One limitation of using LLMs to gener- that does not align with expectations (e.g. Figures
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“CHiLL: Zero-shot Custom Interpretable Feature Extraction from Clinical Notes with Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
