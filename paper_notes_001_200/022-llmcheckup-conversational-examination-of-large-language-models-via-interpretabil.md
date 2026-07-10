# 022. LLMCheckup: Conversational Examination of Large Language Models via Interpretability Tools and Self-Explanations

> 逐篇阅读记录：第 22 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Qianli Wang, Tatiana Anikina, Nils Feldhus, Josef van Genabith, Leonhard Hennig, Sebastian Möller
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.hcinlp-1.9)
- **领域标签**：Rationale, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 8,961 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Qianli Wang, Tatiana Anikina, Nils Feldhus, Josef Genabith, Leonhard Hennig, Sebastian Möller.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction explainability methods, as they may not be aware；4 Use cases knowledge and leveraging it to provide accurate；6 Discussion；2022. Chain of thought prompting elicits reasoning
- **引言关键线索**：of which techniques are available or how to inter- To unravel the black box nature of deep learning pret results provided. There has been a consen- models for natural language processing, a diverse sus within the research community that moving range of explainability methods have been devel- beyond one-off explanations and embracing con- oped (Ribeiro et al., 2016; Madsen et al., 2022; versations to provide explanations is more effective Wiegreffe et al., 2022). Nevertheless, practition- for model understanding (Lakkaraju et al., 2022; ers often face difficulties in effectively utilizing Feldhus et al., 2023; Zhang et al., 2023) and helps *Equal contribution mitigate the limitations associated with the effec- 1 https://github.com/DFKI-NLP/LLMCheckup tive usage of explainability methods to some extent 89 Proceedings of the Third Workshop on Bridging Human鈥揅omputer Interaction and Natural 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：box explainability tools such as feature attribu- tions, and self-explanations (e.g., for rationale generation). LLM-based (self-)explanations are presented as an interactive dialogue that supports follow-up questions and generates sug- gestions. LLMCHECKUP provides tutorials for operations available in the system, catering to Figure 1: LLMCHECKUP dialogue with data augmenta- individuals with varying levels of expertise in tion and rationalization operations on a commonsense XAI and supporting multiple input modalities. question answering task (ECQA). Boxes (not part of the We introduce a new parsing strategy that sub- actual UI) indicate the original instance from the dataset stantially enhances the user intent recognition as well as its prediction (cyan) and the explanation re- accuracy of the LLM. Finally, we showcase quested by the user (orange). LLMCHECKUP for the tasks of fact checking and commonsense question answering.
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Customized inputs, In comparison to, ECQA task. Our, We present the interpretability, LLMCHECKUP, Mosca et al
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Customized inputs & prompts In comparison to
  - baseline, we employ the nearest neighbor approach the increased complexity of the ECQA task. Our
  - F1 , precision, recall and accuracy matrices. we have intentionally omitted providing a baseline.
  - model comparison and error analysis across var- We present the interpretability tool LLMCHECKUP,
  - (Mosca et al., 2023) enables real-time explanation- ules or libraries and serves as a baseline for future

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - accuracy of the LLM. Finally, we showcase quested by the user (orange).
  - We showcase a short dialogue between the user
  - attributes can improve performance. we remove it from the parser output. 搂5.1 evaluates
  - Pythia 10 75.91 quantized Llama model outperforms its full (non-
  - exhibits the capability to facilitate quick cross- 8 Conclusion
  - planations in a conversational manner can improve GINGCHAT12 would further increase the visibility
  - tions for improved use and mental models of natural Computational Linguistics: ACL 2023, pages 7555鈥

## 6. Conclusion、局限与可复现性

- **结论段落线索**：provement over GD. Despite Stable Beluga 2 having a larger size compared to 7B models, its In contrast to previous dialogue-based XAI frame- parsing performance only marginally surpasses that works CONVXAI (Shen et al., 2023) and INTER- of Mistral and Llama2. This can be partially at- ROLANG (Feldhus et al., 2023), which require a tributed to the difficulty of the parsing task10 and fine-tuned model for each specific use case, LLMs the number of demonstrations, as larger models used in LLMCHECKUP possess remarkable zero- may require a greater number of demonstrations to /few-shot capabilities (Brown et al., 2020) for ef- fully comprehend the context (Li et al., 2023b). fectively handling many tasks without requiring Table 3 summarizes our parsing evaluation re- fine-tuning. Although the quality of an explana- sults for different models with different number tion could be enhanced with further fine-tuning, of 鈥檓ax_new_tokens鈥 for generation. Llama-based LLMCHECKUP uses model outputs out
- **局限/未来工作线索**：*Equal contribution mitigate the limitations associated with the effec-；dictory claims that have been debunked by the pre- as that aspect is considered as future work and falls outside；to the sequence-to-class format, restricting its ap- Future work includes exploring RAG models；interactive interrogation in order to build under- Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“LLMCheckup: Conversational Examination of Large Language Models via Interpretability Tools and Self-Explanations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
