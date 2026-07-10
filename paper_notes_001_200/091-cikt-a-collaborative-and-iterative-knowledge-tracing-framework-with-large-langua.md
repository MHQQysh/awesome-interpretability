# 091. CIKT: A Collaborative and Iterative Knowledge Tracing Framework with Large Language Models

> 逐篇阅读记录：第 91 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：R N Li, Shuqun Wu, Jun Wang, Wei Zhang
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.975)
- **领域标签**：Representation, Evaluation, Hidden, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 9,147 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Knowledge Tracing (KT) aims to model a student's learning state over time and predict their future performance.However, traditional KT methods often face challenges in explainability, scalability, and effective modeling
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Methodology dergo a manual curation process where their format；1. Overall Performance；4. Addition；1. Profile Generation: The current Analyst, pa- (11)；4 Experiments；2009. Addressing the assessment challenge with In IEEE/WIC/aCM international conference on web；2023. Mlagentbench: Evaluating language agents on ing, Adaptation, and Personalization: 18th Interna-；2021. Learning process-consistent knowledge trac-
- **引言关键线索**：ories (Chen et al., 2023; Cui et al., 2024)鈥攎any Knowledge Tracing (KT) (Corbett and Anderson, DLKT models remain substantially opaque. This 1994) is a foundational task in educational data lack of transparency can hinder their adoption and mining and intelligent tutoring systems, aiming trustworthiness in high-stakes educational settings to model a student鈥檚 evolving knowledge state where understanding the model鈥檚 reasoning is cru- from their historical learning interactions to ac- cial. curately predict future performance, thereby fa- The transformative capabilities of LLMs, demon- * Corresponding authors strated across specialized domains like scien- 19321 Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing, pages 19321鈥19334 November 4-9, 2025 漏2025 Association for Computational Linguistics tific discovery (Pyzer-Knapp et al., 2022; Mer- CIKT framew
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Large Language Models Runze Li鈥 , Siyu Wu鈥 , Jun Wang鈥 * , Wei Zhang鈥犫 * 鈥 School of Computer Science and Technology, East China Normal University 鈥 Shanghai Innovation Institute 51265901021@stu.ecnu.edu.cn, 52265901039@stu.ecnu.edu.cn wongjun@gmail.com, zhangwei.thu2011@gmail.com Abstract cilitating personalized learning and targeted inter- ventions. While early approaches like Bayesian Knowledge Tracing (KT) aims to model a stu- dent鈥檚 learning state over time and predict their Knowledge Tracing (BKT) (Corbett and Ander- future performance. However, traditional KT son, 1994) and its extensions (Pardos and Hef- methods often face challenges in explainability, fernan, 2011, 2010) offered interpretable param- scalability, and effective modeling of complex eters, they often struggled with the complex tem- knowledge dependencies. While Large Lan- poral dependencies of learning processes. Deep guage Models (LLMs) present new avenues learnin
- **方法与解释性关系**：该论文主要围绕 `Representation, Evaluation, Hidden, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：ASSIST12, Feng et al, Also from the, Prior to model training, Baselines, Data Cleaning, Invalid or duplicate records, Deepseek R1, Guo et al, LLM baselines, KT and several LLM, Detailed, For model evaluation, KT baselines in terms, Ac-, LLM baselines like
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - guage model baselines? 鈥 ASSIST12 (Feng et al., 2009): Also from the
  - Prior to model training, the datasets underwent 4.1.4 Baselines
  - 鈥 Data Cleaning: Invalid or duplicate records, work, we compare its performance against a di-
  - such as those lacking essential information (e.g., verse set of nine baselines, covering both main-
  - ensure effective modeling length. Deepseek R1 (Guo et al., 2025) as LLM baselines.
  - tor components leverage large language models as traditional KT and several LLM baselines. Detailed

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - for KT, their direct application often struggles emerged, significantly advancing predictive accu-
  - demonstrates significant improvements in pre-
  - diction accuracy, offers enhanced explainability Despite these significant strides in predictive
  - and exhibits improved scalability. Our work persistent challenge in the DLKT landscape (Minn
  - tific discovery (Pyzer-Knapp et al., 2022; Mer- CIKT framework outperforms existing KT mod-
  - operate statically post-deployment, lacking mech- et al., 2009) aimed to improve accuracy and flexibil-
  - anisms for continuous self-improvement based on ity. However, these models struggled with complex

## 6. Conclusion、局限与可复现性

- **结论段落线索**：informative signals for the Predictor, enabling more We proposed CIKT to iteratively optimize student faithful and transparent forecasting. profiling and performance prediction for accurate Beyond qualitative analysis, we conducted a and explainable knowledge tracing. Its synergistic blind human evaluation to quantitatively validate architecture enables continuous mutual enhance- the utility of the generated profiles for explainabil- ment, yielding significantly improved predictive ity. Two expert annotators performed a forced- accuracy and explainable student profiles on multi- choice comparison on 100 profile pairs (initial vs. ple educational datasets. refined) produced by CIKT-Qwen2.5-7B on the ASSIST2009 dataset, deciding in each case which Limitations profile was better. The preferences were highly consistent: both annotators independently agreed Because our framework primarily generates bina- that the refined profile was superior in 82% of the rized judgments for student respons
- **局限/未来工作线索**：To address these multifaceted limitations, we Deep learning-based knowledge tracing (DLKT)；propose Collaborative Iterative Knowledge Tracing models emerged to overcome these limitations.；ing, a common limitation is their static operation rectness. The LLMteacher produces initial textual；ASSIST2009 dataset, deciding in each case which Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“CIKT: A Collaborative and Iterative Knowledge Tracing Framework with Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
