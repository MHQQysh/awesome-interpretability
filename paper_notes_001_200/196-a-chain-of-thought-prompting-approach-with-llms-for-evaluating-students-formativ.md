# 196. A Chain-of-Thought Prompting Approach with LLMs for Evaluating Students’ Formative Assessment Responses in Science

> 逐篇阅读记录：第 196 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Clayton Cohn, Nicole Hutchins, Tuan Anh Le, Gautam Biswas
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/30364/32416)
- **领域标签**：Rationale, Reasoning, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 7,878 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：This paper explores the use of large language models (LLMs) to score and explain short-answer assessments in K-12 science.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2. Ability to Leverage the Model to Support Rubric Re-；3. Resolve Unexplained Model Applications: In some；2020. From Theory to Action: Developing and Evaluating
- **引言关键线索**：and problem-solving processes (Hutchins and Biswas 2023). Improvements in Science, Technology, Engineering, and In these environments, knowledge and skill development Mathematics (STEM) education have accelerated the shift happen through system interactions that are difficult to mon- from teaching and assessing facts to developing stu- itor and interpret (Walkoe, Wilkerson, and Elby 2017). For- dents鈥 conceptual understanding and problem-solving skills mative assessments, evaluation, and feedback mechanisms (NGSS 2013). To foster students鈥 developing scientific ideas aligned with target learning goals (Bloom, Madaus, and and reasoning skills, it is crucial to have assessments that re- Hastings 1971), can play a dual role: (1) help students rec- veal and support their progress (Harris et al. 2023). Forma- ognize constructs that are important to learning, and (2) pro- tive assessments play
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Formative Assessment Responses in Science Clayton Cohn1 , Nicole Hutchins1 , Tuan Le2 , Gautam Biswas1 1 Vanderbilt University 2 DePauw University {clayton.a.cohn, nicole.m.hutchins, gautam.biswas}@vanderbilt.edu, tuanle 2025@depauw.edu Abstract This paper develops an approach for human-in-the-loop LLM prompt engineering using in-context learning and This paper explores the use of large language models (LLMs) to score and explain short-answer assessments in K-12 sci- chain-of-thought reasoning with GPT-4 to support auto- ence. While existing methods can score more structured math mated analysis and feedback generation for formative as- and computer science assessments, they often do not provide sessments in a middle school Earth Science curriculum. We explanations for the scores. Our study focuses on employ- present our approach, discuss our results, evaluate the limi- ing GPT-4 for automated assessment in middle school Earth tations of
- **方法与解释性关系**：该论文主要围绕 `Rationale, Reasoning, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 1, Performance comparisons for the, Q1 Arrow Size Zero-Shot, Chain-of-Thought Few-Shot, CoT 5 0.94 0.77, Zero-Shot baseline, Few-Shot baseline, Zero-Shot 0 0.60 0.59, Few-Shot, CoT, CoT rea- CoT +, AL 10 0.85 0.79, Chain-of-Thought Prompting + Active, Learning approach Table 2, Performance comparisons for Question, Evaluating our ap-, Model performance comparisons for, For Arrow Size
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Table 1: Performance comparisons for the Q1 Arrow Size Zero-Shot 0 0.92 0.73 0.47
  - three incremental baselines, and our Chain-of-Thought Few-Shot, CoT 5 0.94 0.77 0.55
  - a Zero-Shot baseline, where the rubric is included in the
  - a Few-Shot baseline, where we provided the model with la- Zero-Shot 0 0.60 0.59 0.65
  - third and final baseline, Few-Shot, CoT, added CoT rea- CoT + AL 10 0.85 0.79 0.87
  - Chain-of-Thought Prompting + Active Learning approach Table 2: Performance comparisons for Question 2.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 point, 4 points, 0.83
points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - explanations for the scores. Our study focuses on employ- present our approach, discuss our results, evaluate the limi-
  - Improvements in Science, Technology, Engineering, and In these environments, knowledge and skill development
  - knowledge, there is very little research that combines au- duced improved automated assessment scoring approaches
  - ternatively, teachers can refine the rubric to improve grading the LLM responses via CoT reasoning. In future work, as
  - improve model output. from updating the rubric during Active Learning; however,
  - needed, refining rubrics and formative assessment questions. to improve readability and allow for programmatic parsing
  - algebraic word problems were broken down step-by-step to distribution can improve performance (Cochran et al. 2022).

## 6. Conclusion、局限与可复现性

- **结论段落线索**：should also be noted that the level of agreement during IRR In this paper, we employed a Chain-of-Thought Prompting + may provide a ballpark expectation of model performance, Active Learning approach for scoring and explaining forma- as we found questions that were easier for the human scorers tive assessment question responses in a middle school Earth to agree on were also easier for the model to correctly align Science curriculum. Our results show that GPT-4, CoT rea- with the human scorers. Similarly, in questions where soning, and active learning can be effectively leveraged to- the human scorers had difficulty achieving consensus, the ward accurate grading of science formative assessments. In model had difficulty with scoring. More research needs to several cases, the model achieved 鈥渁lmost perfect鈥 align- be done to evaluate this quantitatively. ment with humans. The model generated relevant evidence linked to the rubric to help explain its scoring, which could Comparing Model an
- **局限/未来工作线索**：ternatively, teachers can refine the rubric to improve grading the LLM responses via CoT reasoning. In future work, as；troduced into the prompt using CoT reasoning to rectify dis- learning. Due to token limitations and the time cost for in-；sponse + reference to the rubric + score. We used quotations learning to help mitigate this in future work. For all instances；and active learning run the risk of overfitting, particularly discussed above provide opportunities for future work to
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“A Chain-of-Thought Prompting Approach with LLMs for Evaluating Students’ Formative Assessment Responses in Science”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
