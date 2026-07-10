# 131. The Troubling Emergence of Hallucination in Large Language Models - An Extensive Definition, Quantification, and Prescriptive Remediations

> 逐篇阅读记录：第 131 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Vipula Rawte, Swagata Chakraborty, Agnibh Pathak, Anubhav Sarkar, S. M Towhidul Islam Tonmoy, Aman Chadha, Amit Sheth, Amitava Das
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.155)
- **领域标签**：Analysis, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 18,492 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Vipula Rawte, Swagata Chakraborty, Agnibh Pathak, Anubhav Sarkar, S.M Towhidul Islam Tonmoy, Aman Chadha, Amit Sheth, Amitava Das.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 AI Institute, University of South Carolina, USA, 2 Christ University, India；3 Islamic University of Technology, Bangladesh；4 Stanford University, USA, 5 Amazon AI, USA；4 Hallucination Vulnerability Index (HVI)；6 Hallucination Mitigation Strategies Replacement (ENTROPYBB ): A；7 Conclusion and Future Avenues；1 Three graduate students
- **引言关键线索**：& mitigations, (viii) evaluation, (ix) testing, (x) cination by (Ladhak et al., 2023b), where a person machine-generated content, (xi) member states, named Jung Lee is falsely attributed with French and (xii) downstream documentation. The over- nationality. Although one could argue that Mr Lee all grading of each LLM can be observed in Fig. 8. may indeed be a French national, considering his While this study is commendable, it appears to birth and upbringing there, the authors confirm be inherently incomplete due to the ever-evolving that no such individual exists. We posit that name- nature of LLMs. Since all scores are assigned man- nationality hallucination falls under the sub-class ually, any future changes will require a reassess- of generated golems. It is plausible that a combina- ment of this rubric, while HVI is auto-computable. tion of our defined categories may exist, although
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：grained discourse on profiling hallucination 1 Hallucination: The What and Why based on its degree, orientation, and category, Orientation Category Degree along with offering strategies for alleviation. As such, we define two overarching orienta- Time Wrap alarming tions of hallucination: (i) factual mirage (FM) Intrinsic and (ii) silver lining (SL). To provide a more Factual Geographic Erratum comprehensive understanding, both orienta- Mirage tions are further sub-categorized into intrinsic Extrinsic Virtual Voice moderate and extrinsic, with three degrees of severity - (i) mild, (ii) moderate, and (iii) alarming. We also meticulously categorize hallucination into Generated Golem six types: (i) acronym ambiguity, (ii) numeric Intrinsic nuisance, (iii) generated golem, (iv) virtual Silver Numeric Nuisance Lining voice, (v) geographic erratum, and (vi) time mild Extrinsic wrap. Furthermore, we curate HallucInation Acronym Ambiguity eLici
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：AI-generated text into tion
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - scores, we categorize the AI-generated text into tion techniques can serve as baselines.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：16x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - nificant concerns. While some recent endeav- HVI holds significant value as a tool for the
  - a limited emphasis on the nuanced categoriza- In conclusion, we propose two solution strate-
  - nation to implement improved controls on hallu- troduce a few new terms. These newly introduced
  - cally demonstrated to outperform majority voting, of data points, 15 in this case. Finally, for ease of
  - LLMs. The resulting HVIs are then ranked and significantly increases from GPT-3.5 to GPT-4.
  - 7 Conclusion and Future Avenues
  - uate a total of n sentences for their relevance to one of the most significant challenges faced by

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tion of hallucination and associated mitigation gies for mitigating hallucinations. methods. To address this gap, we offer a fine- grained discourse on profiling hallucination 1 Hallucination: The What and Why based on its degree, orientation, and category, Orientation Category Degree along with offering strategies for alleviation. As such, we define two overarching orienta- Time Wrap alarming tions of hallucination: (i) factual mirage (FM) Intrinsic and (ii) silver lining (SL). To provide a more Factual Geographic Erratum comprehensive understanding, both orienta- Mirage tions are further sub-categorized into intrinsic Extrinsic Virtual Voice moderate and extrinsic, with three degrees of severity - (i) mild, (ii) moderate, and (iii) alarming. We also meticulously categorize hallucination into Generated Golem six types: (i) acronym ambiguity, (ii) numeric Intrinsic nuisance, (iii) generated golem, (iv) virtual Silver Numeric Nuisance Lining voice, (v) geographic erratum, and (vi) time 
- **局限/未来工作线索**：8 Discussion and Limitations Numeric Nuisance present in the shown sentence.；this study, the authors put forward a grading sys- Limitation 2: While we have meticulously de-；(v) energy, (vi) capabilities & limitations, (vii) risk of this is the introduction of name-nationality hallu-；sidered the most suitable category for assessing Limitation 3: For this study, we have chosen
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“The Troubling Emergence of Hallucination in Large Language Models - An Extensive Definition, Quantification, and Prescriptive Remediations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
