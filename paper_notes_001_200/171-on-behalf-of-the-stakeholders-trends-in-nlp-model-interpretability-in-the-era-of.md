# 171. On Behalf of the Stakeholders: Trends in NLP Model Interpretability in the Era of LLMs

> 逐篇阅读记录：第 171 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Nitay Calderon, Roi Reichart
- **发表 venue / date**：NAACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.naacl-long.29)
- **领域标签**：Analysis, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 26,969 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Nitay Calderon, Roi Reichart.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：700 Humanities；400 Natural Sci 100；1 Introduction；3. The Scientific Perspective: Language is tightly；4. The Social Perspective: addresses the broader restrict the interpretability method to explain the；5 Trends in Model Interpretability；2021. Causalm: Causal model explanation through Faithful explanations of black-box nlp models using；2022. Shared computational principles for language
- **引言关键线索**：125 led to widespread adoption of these systems Economics
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：the practical implications of these paradigms by counted, following relevance filtering by an LLM. analyzing trends from the past decade across multiple research fields. To this end, we re- Shamszare, 2023; Kasneci et al., 2023; von Garrel trieved thousands of papers and employed an and Mayer, 2023). However, these black-box mod- LLM to characterize them. Our analysis reveals els are complex and opaque (Wallace et al., 2019; significant disparities between NLP developers Calderon et al., 2023; Luo et al., 2024). While per- and non-developer users, as well as between formance has advanced, this comes at the cost of research fields, underscoring the diverse needs understanding their underlying mechanisms (Lyu of stakeholders. For example, explanations of internal model components are rarely used out- et al., 2022; Madsen et al., 2023; Singh et al., 2024). side the NLP field. We hope this paper informs The ability to explain decisions is p
- **方法与解释性关系**：该论文主要围绕 `Analysis, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：未自动提取到稳定短句，需回看论文中的 Baselines、Comparison 或 Ablation 小节。

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significant disparities between NLP developers Calderon et al., 2023; Luo et al., 2024). While per-
  - of methods that align with the objectives and significantly influence individual decision-making
  - that align with the objectives, expectations, and ing needs, reflected in significant differences
  - does not significantly differ between two subpopu-
  - contain human biases and prejudices (Blodgett to improve or modify their functionality.
  - ual decisions. Conversely, in neuroscience and so- 6 Conclusions and Recommendations
  - tically improved the capabilities of NLP models. holders鈥 perspective. Additionally, we conducted a

## 6. Conclusion、局限与可复现性

- **结论段落线索**：and how parts (see the paragraph below the follow- values is an interpretability method, while visualiz- ing definition), we must first define an explanation. ing these values using the SHAP Python package2 This is because the what and how are derived from and providing guidance on interpreting these visu- the why 鈥 the stakeholders, and clearly, they are alizations constitute an explanation. part of an explanation. To this end, we have gath- ered common (though not formal) definitions from 4 Properties and Categorization seminal works in the literature (Doshi-Velez and Kim, 2017; Lipton, 2018; Murdoch et al., 2019; In this section, we propose and briefly describe Arrieta et al., 2020; Lyu et al., 2022; R盲uker et al., properties that answer the what and how questions 2023), and propose the following definition: derived from our interpretability definitions. In Ta- ble 1, we present a categorization of interpretability Explanation (explaining): paradigms based on the properties. In Appe
- **局限/未来工作线索**：7 Limitations 24-26, 2017, Conference Track Proceedings. Open-；Linguistics, COLING 1990, University of Helsinki, language models: Challenges, limitations, and rec-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“On Behalf of the Stakeholders: Trends in NLP Model Interpretability in the Era of LLMs”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
