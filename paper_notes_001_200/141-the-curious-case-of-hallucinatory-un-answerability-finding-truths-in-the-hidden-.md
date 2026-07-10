# 141. The Curious Case of Hallucinatory (Un)answerability: Finding Truths in the Hidden States of Over-Confident Large Language Models

> 逐篇阅读记录：第 141 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Aviv Slobodkin, Omer Goldman, Avi Caciularu, Ido Dagan, Shauli Ravfogel
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.220)
- **领域标签**：Representation, Rationale, Hallucination, Hidden, Behavior, Analyze
- **本地 PDF 文本规模**：约 13,640 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Large language models (LLMs) have been shown to possess impressive capabilities, while also raising crucial concerns about the faithfulness of their responses.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Method each task, each model is prompted with a bal-；4 Experimental Setup select the paragraph that exhibits the highest cosine-；6 Conclusion the information we are interested in in a nonlinear；2019 Conference on Empirical Methods in Natu-；2. Given the following passage and question, an- Table 7: Exact match and (token) F1 scores in the zero-；3. Given the following passage and question, an-
- **引言关键线索**：embedding of Flan-UL2 on each of the three bench- Modern large language models (LLMs) have been marks. The left images show the embeddings with tantalizing the NLP community in the last couple the regular prompt, and the right ones 鈥 with a hint- including prompt. Blue and red dots are examples of years (Brown et al., 2020; Chen et al., 2021; correctly detected by the model as answerable and Chung et al., 2022), demonstrating great potential (un)answerable, respectively, while the pink dots are for both research and commercial use, but these for (un)answerable examples that the model provided models are of course not problem-free. Among answers to. The figures show the good separability be- their unfavorable behaviors it is possible to find tween the three groups. toxicity (Welbl et al., 2021; Deshpande et al., 2023), bias (Nadeem et al., 2021; Abid et al., 2021), and hallucination (Mana
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：December 6-10, 2023 漏2023 Association for Computational Linguistics (Jiang et al., 2021; Kadavath et al., 2022). We, how- of (un)answerable questions, in which supporting ever, ask whether models already represent ques- paragraphs have been intentionally removed from tions鈥 (un)answerablility when producing answers, the context. Our experiments use these datasets to and find strong evidence for a positive answer. demonstrate the effectiveness of our approach to Specifically, by experimenting with three QA identify (un)answerability. datasets (Rajpurkar et al., 2018; Kwiatkowski et al., (Un)answerability capabilities in LLMs were 2019; Trivedi et al., 2022), we observe a substan- mainly studied by using few-shot prompting (Kand- tial increase in performance for (un)answerable pal et al., 2022; Weller et al., 2023). Moreover, sev- questions (up to 80%) simply by incorporating eral works have recently shown that LLMs become to the prompt t
- **方法与解释性关系**：该论文主要围绕 `Representation, Rationale, Hallucination, Hidden, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Appendix D for a
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - a baseline, we also conduct similar experiments
  - Appendix D for a comparison of the two possible

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：80 points, 50 points, 8.7 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - tory answer? Our results show strong indica-
  - for the development of improved decoding tech-
  - ods: first, we find that the beam of decoded re- 2023a), and as a consequence, it might improve the
  - 2022) to improve performance in general and on Furthermore, recent studies have suggested uti-
  - MuSiQue (Trivedi et al., 2022) was introduced as a work, as they suggest to analyze and improve the
  - Conversely, for the (un)answerable queries, the ab- (un)answerable questions is substantially improved
  - task we only examine whether they tried to an- in improvements of over 50 points (on both met-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：manner. As such, our results should only be inter- We found ample evidence for LM鈥檚 ability to en- preted as a lower bound for the identification of code the (un)answerability of questions, despite the (un)answerability. Lastly, our experiments focused fact that models tend to be over-confident and gen- on (un)answerability in a given context. Future erate hallucinatory answers when presented with work should also explore the phenomenon in the (un)answerable questions. We also showed that this open-domain setting. discrepancy between model output and its hidden states is mitigated by simply adding the option of 8 Ethics Statement (un)answerability to the prompt. The evidence we found includes the existence Model hallucination, in general, can have real- of a reply acknowledging the (un)answerability in world implications when models are incorporated a beam of decoded answers, meaning that even in, e.g., search engines or other applications. Our though the models鈥 best-assessed answer i
- **局限/未来工作线索**：While linear erasure has its limitations (Ravfogel from a single paragraph.；formance in the answerability task. Indeed, when 7 Limitations；ing Surveys, 55(12):1鈥38. tigating effectiveness and limitations of paramet-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“The Curious Case of Hallucinatory (Un)answerability: Finding Truths in the Hidden States of Over-Confident Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
