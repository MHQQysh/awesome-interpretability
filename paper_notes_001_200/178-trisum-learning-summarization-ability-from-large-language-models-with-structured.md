# 178. TriSum: Learning Summarization Ability from Large Language Models with Structured Rationale

> 逐篇阅读记录：第 178 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Pengcheng Jiang, Cao Xiao, Zifeng Wang, Parminder Bhatia, Jimeng Sun, Jiawei Han
- **发表 venue / date**：NAACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.naacl-long.154)
- **领域标签**：Rationale, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 11,917 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Pengcheng Jiang, Cao Xiao, Zifeng Wang, Parminder Bhatia, Jimeng Sun, Jiawei Han.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction LLMs has been employed for NLP tasks like QA,；2 Related Work；3 Method；4 Experiments；47 ROUGE-1 25 ROUGE-2 43 ROUGE-L；0 Singular-task Learning 1 Concurrent Learning 2 Joint Learning 3；2022. Sequence level contrastive learning for text is contingent on the quality and capabilities of the
- **引言关键线索**：Large language models (LLMs), such as GPT-3 natural language understanding (NLU), and arith- (Brown et al., 2020) and its successors (Chowdhery metic reasoning (Wang et al., 2022; Hsieh et al., et al., 2022; Touvron et al., 2023; OpenAI, 2023), 2023; Magister et al., 2023; Ho et al., 2023), its has greatly advanced natural language processing applicability to abstractive text summarization re- tasks, including machine translation (Brants et al., mains unexplored. 2007), question-answering (QA) systems (Yang In this study, we aim to distill LLMs鈥 text sum- et al., 2019; Bao et al., 2021), and text summa- marization prowess into a more compact local rization (Liu and Lapata, 2019). However, due model. We enhance the transparency and inter- to their substantial model size and computational pretability of this local model by incorporating demands, their utility can be limited in resource- el
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：abilities into a compact, local model. Initially, with LLM鈥檚 text summarization capability. LLMs extract a set of aspect-triple rationales and summaries, which are refined using a dual- However, many existing methods struggle to gen- scoring method for quality. Next, a smaller erate structured summaries (Brown et al., 2020; local model is trained with these tasks, employ- Gekhman et al., 2023; Liu et al., 2023). These struc- ing a curriculum learning strategy that evolves from simple to complex tasks. Our method tured summaries need to encompass essential as- enhances local model performance on various pects, key entities and relationships, and a coherent benchmarks (CNN/DailyMail, XSum, and Clin- final summary derived from these aspects and ratio- icalTrial), outperforming baselines by 4.5%, nales. Recent developments have seen the utiliza- 8.5%, and 7.4%, respectively. It also improves tion of LLMs to grasp a text鈥檚 topic structure an
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Recent developments have seen, However, Baselines We compare TriSum, The results include both, ROUGE, Table 2, Performance comparison of ROUGE, Scores across CNN, DailyMail, XSum, ClinicalTrial, Consistent Edge Over Baselines, The TriSum the CNNDM, LLM, Gains Over Backbone We, BART as the task, Two key comparisons are, Figure 3
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - icalTrial), outperforming baselines by 4.5%, nales. Recent developments have seen the utiliza-
  - in developing a baseline understanding and abil- informs the next. However, a challenge emerges
  - and summary generations. cluding the baselines, undergo fine-tuning for three
  - rationales and summaries. Baselines We compare TriSum to baseline ab-
  - https://clinicaltrials.gov/ baseline models. The results include both ROUGE
  - Table 2: Performance comparison of ROUGE Scores across CNN/DailyMail, XSum, and ClinicalTrial

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：4.5 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - has significantly advanced natural language
  - icalTrial), outperforming baselines by 4.5%, nales. Recent developments have seen the utiliza-
  - 8.5%, and 7.4%, respectively. It also improves tion of LLMs to grasp a text鈥檚 topic structure and
  - 鈥 Through extensive experiments we show that in- idea has been applied and extended across various
  - have improved the quality of text summarization informs the development of better knowledge dis-
  - significantly. These models excel at capturing tillation methods. (Wang et al., 2022) developed a
  - improvement in the aggregate ROUGE scores achieved by TriSum-J. The top-3 results are highlighted. Our

## 6. Conclusion、局限与可复现性

- **结论段落线索**：This underscores the importance of the quality of rationales; poor-quality rationales can negatively We introduced TriSum, an approach aimed at dis- impact the model, emphasizing the value of our tilling summarization capabilities from a large lan- rationale selection strategy. guage model to a small local model. Extensive experiments verified its superior performance over Case Study Figure 3 compares summaries cre- state-of-the-art models across diverse datasets on ated from a CNN article discussing an oil rig fire the abstractive summarization task. Our work high- in Mexico. The ground truth summary adeptly lights the potential of leveraging large model in- encapsulates the main events, emphasizing the af- sights for efficient and nuanced text summarization. 2812
- **局限/未来工作线索**：Linguistics. A.2 Limitations；A Ethics, Limitations, and Risks to (1) generate essential aspects of the document
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“TriSum: Learning Summarization Ability from Large Language Models with Structured Rationale”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
