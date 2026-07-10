# 052. Characterizing Large Language Models as Rationalizers of Knowledge-intensive Tasks

> 逐篇阅读记录：第 52 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Aditi Mishra, Sajjadur Rahman, Kushan Mitra, Hannah Kim, Estevam Hruschka
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.484)
- **领域标签**：Rationale, Evaluation, Hallucination, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 14,116 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Large language models (LLMs) are proficient at generating fluent text with minimal taskspecific supervision.However, their ability to generate rationales for knowledge-intensive tasks (KITs) remains under-explored.Genera
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction izing the suitability of LLMs as rationalizers of；5 Acceptability of LLM Rationalization 8 CSQA OBQA；7 Related Work；8 Conclusion；9 Limitations；2023. Selfcheckgpt: Zero-resource black-box hal-；003. While we show only two few-shot examples, tion and the knowledge-guided LLM rationaliza-
- **引言关键线索**：knowledge-intensive task (KIT) decisions such as In recent years, generating rationales (i.e., free- commonsense question answering (CSQA (Talmor text explanations) of natural language understand- et al., 2019)) and open book question answering ing tasks has been increasingly explored in the (OBQA (Mihaylov et al., 2018)) requires further field of explainable NLP. Such rationales 鈥 while investigation due to the difference in scope and less functionally grounded, i.e., they may not en- setting from prior work (Wiegreffe et al., 2022). tirely reflect the model鈥檚 behavior 鈥 provide an Firstly, KITs such as CSQA and OBQA are effective interface to interpretably communicate framed as multiple-choice questions, requiring model decisions to end-users (Hendricks et al., models to select one answer from several choices 2016; Camburu et al., 2018; Madsen et al., 2022; (see Figure 1a). Therefore, 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：able NLP omit the incorrect prediction confounder nalizing and find that even simple strategies may and evaluate only rationales of correct predictions. help intervene up to 71% of the incorrect predic- However, with LLM-generated rationales being in- tions. The code and data related to the human- creasingly adopted in real-world scenarios, such subject studies are publicly available2 . The key as rationalizing why a candidate is suitable for an contributions of our work include: advertised job1 , it is important to scrutinize the 鈥 design of three human-subject studies to eval- practical implications of such deployments and in- uate free-text rationales on previously unex- form guidelines for safe and responsible adoption. plored aspects (such as trust and reliability) Given the setting of generating corroborating while adapting existing studies for the RAG- and refutation complete rationales of KIT model based LLM rationalization sett
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Hallucination, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Renner et al, Fine-grained comparison. Besides head-to-head, NLP, LLM- comparison, Likert scale, Fine-grained Comparison, LLM-, Comparisons between mturk, Computational Linguistics, ECQA rationale, Bean bag chair is, HITs in to-, For both studies, CSQA and OBQA datasets
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - inspired by existing work on trust in explainable ment statistics (伪 鈭 [0.05, 0.20]) on comparison
  - Renner et al., 2020), we conducted a confirmatory Fine-grained comparison. Besides head-to-head
  - study in the context of explainable NLP (i.e., LLM- comparison, we ask several 7-point Likert scale
  - 4.3 Fine-grained Comparison
  - bination of semantic similarity measures and natu- rationales in a head-to-head comparison, LLM-
  - subjects research: Comparisons between mturk, pro- Computational Linguistics.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - nales highlights the need for further improve- ability to different domains (Wiegreffe and Maraso-
  - prediction accuracy into account. We find that challenges while showcasing surprising effective-
  - the answer (Aggarwal et al., 2021). We show an ten than not, crowdworkers prefer LLM-generated
  - rationales in prior work are abstractive (Gurrapu nificant room for improvement along dimensions
  - accounting for order effect of tasks and individual face details in Appendix E.1.) We find low-to-
  - ECQA rationales are reported to be overall better The result showcases an improvement over previ-
  - written ECQA rationales potentially outperformed Conciseness 0.08 0.02

## 6. Conclusion、局限与可复现性

- **结论段落线索**：convincingness and objective properties, such as conciseness, of a rationale. Since the effective- We evaluate LLMs鈥 capacity to generate effective ness of free-text rationales lies in natural language- rationales for knowledge-intensive tasks in a few- based seamless communication to end-users, in this shot knowledge-guided setting. We additionally work, we prioritized characterizing LLM-generated investigate the implications of employing LLMs as rationales of model decisions communicated to end- rationalizers of an imperfect model and highlight users. Therefore, we opted for human-subject stud- the negative impact on users鈥 trust. Observations ies that aim to scrutinize the utility of such ratio- from our studies highlight room for improvement nales for knowledge-intensive tasks, characterize in aspects such as task and domain invariant ra- their strengths and limitations, and inform guide- tionalization and robust intervention strategies for lines for safe and responsible adoption. 
- **局限/未来工作线索**：their strengths and limitations, and inform guide- tionalization and robust intervention strategies for；work for evaluating XAI tools with respect to their budget limitations. To this end, existing automated；against such issue. Future work may explore dif-；the subjectivity of the QA tasks. Future work may Subbiah, Jared D Kaplan, Prafulla Dhariwal, Arvind
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Characterizing Large Language Models as Rationalizers of Knowledge-intensive Tasks”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
