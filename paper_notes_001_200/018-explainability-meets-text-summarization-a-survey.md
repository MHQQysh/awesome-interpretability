# 018. Explainability Meets Text Summarization: A Survey

> 逐篇阅读记录：第 18 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Mahdi Dhaini, Ege Erdoğan, Smarth Bakshi, Gjergji Kasneci
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.inlg-main.49)
- **领域标签**：Rationale, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 9,355 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Summarizing long pieces of text is a principal task in natural language processing with Machine Learning-based text generation models such as Large Language Models (LLM) being particularly suited to it.Yet these models a
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Background RQ3: How are such explanations visualized and；1144. Automatic Identification of Hallucinations in Large；2024. Knowledge-enhanced graph topic trans-
- **引言关键线索**：the model鈥檚 prediction, and a summary explains Against the ever-growing influx of textual con- the summarized piece of text. It is thus beneficial tent, being able to effectively summarize long to consider the two problems together since ap- pieces of text is crucial to extract useful informa- proaches to one can inform the approaches to the tion. Whereas once a significant amount of manual other, as we will provide examples throughout the labour would be necessary, now automatic text sum- survey. marization (ATS) can be performed by deep learn- Contributions As far as we know, this work is ing models, especially as they grow in capabilities the first to present an overview of explainable text and become more easily accessible (Bubeck et al., summarization and to offer a dual perspective on 2023). Nevertheless, such deep learning models how explainability and summarization can mutu- are 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：understand summarization methods. Explainability Summarizing long pieces of text is a principal in summarization can take two forms, each target- task in natural language processing with Ma- chine Learning-based text generation models ing different stakeholders. The first form involves such as Large Language Models (LLM) being explaining the output of summarization models, in- particularly suited to it. Yet these models are tended for the end users of summarization systems. often used as black-boxes, making them hard The second form is focused on understanding and to interpret and debug. This has led to calls interpreting the internal workings and mechanisms by practitioners and regulatory bodies to im- of the summarization model, primarily aimed at prove the explainability of such models as they debugging the model, which is intended for model find ever more practical use. In this survey, we present a dual-perspective review of the int
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Comparison to ground truth, Two of the earlier, XAI are pre- claim, Our find-
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Comparison to ground truth: ground truth terest in integrating inherent interpretation within
  - Two of the earlier baseline surveys in XAI are pre- claim to cover all the related literature. Our find-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - employed to improve explanations. to extract the essential bits of a longer piece of
  - tion. Whereas once a significant amount of manual other, as we will provide examples throughout the
  - 鈥 We discuss and draw conclusions on the prac- mulated our research questions with a high degree
  - important problem in NLP around creating short RQ4: Can we derive practical conclusions on
  - marization holds significant potential for further ity is applied to text summarization. Our aim is
  - Insights: The significantly higher use of local
  - tain imperceptible perturbations, and thus might ing BERTSum, and reported improvements in the

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tical usefulness of explainability approaches of specificity as follows: in text summarization. RQ1: What are the popular models, datasets, 鈥 We highlight the popular models, datasets, and and evaluation metrics used in existing research on evaluation metrics for text summarization in explainable text summarization? the reviewed papers. RQ2: What XAI techniques are employed for text summarization in the existing research studies?
- **局限/未来工作线索**：three experts evaluating the explanations of sum- ify model behavior. In this direction, future work；to enhance the interpretability of generative models 7 Limitations；Future work: To address the urgent need to Luca Bacco, Andrea Cimino, Felice Dell鈥橭rletta, and；plainability methods applied to ATS, future work Sixth International Workshop on eXplainable SENTI-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Explainability Meets Text Summarization: A Survey”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
