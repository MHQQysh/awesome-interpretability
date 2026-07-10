# 023. The Probabilities Also Matter: A More Faithful Metric for Faithfulness of Free-Text Explanations in Large Language Models

> 逐篇阅读记录：第 23 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Noah Siegel, Oana-Maria Camburu, Nicolas Heess, María Pérez‐Ortiz
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-short.49)
- **领域标签**：Rationale, Reasoning, Hallucination, Behavior, Evaluate
- **本地 PDF 文本规模**：约 13,019 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：In order to oversee advanced AI systems, it is important to understand their underlying decision-making process.When prompted, large language models (LLMs) can provide natural language explanations or reasoning traces th
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Work；1. It does not test whether impactful features are；3. An explanation mention measure: a function vention, so that TVD measures the absolute change；4 Experiments to predicted classes, which we use for computing；5 Results；2022. Selection-inference: Exploiting large language；2023. The alignment problem from a deep learning
- **引言关键线索**：In many applications of ML systems it is important 4. We run experiments with the Llama2 family to understand why the system came to a particular of LLMs on three datasets and demonstrate answer (Rudin, 2018), and the field of explainable that CCT captures faithfulness trends that the AI attempts to provide this understanding. How- existing faithfulness metric used in CT misses. ever, relying on subjective human assessment of ex-
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：planations ), free-text (also called natural language 鈥渆xplainability鈥 in this work. See Appendix B for explanations or NLEs), and structured. Prior work further discussion. on faithfulness has mostly focused on highlights and NLEs. We chose to focus on NLEs in this work 2.2 The Counterfactual Test because highlight-based explanations are highly re- In order to measure whether an explanation cap- strictive in what they can communicate (Camburu tures the true factors responsible for a model鈥檚 pre- et al., 2021; Wiegreffe et al., 2020), while NLEs diction, we need to know which factors are relevant. allow models to produce justifications that are as However, deep neural networks like LLMs are of- expressive as necessary (e.g. they can mention to ten difficult to interpret (Fan et al., 2020). background knowledge that is not present in the To address this problem, Atanasova et al. (2023) input but that the model made use of for its predic-
- **方法与解释性关系**：该论文主要围绕 `Rationale, Reasoning, Hallucination, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - explanation baseline: empty explanations would yield 100% unfaithfulness, while explanations simply repeating all

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1X, 2 x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - model鈥檚 predictions. In this work, we introduce nations mention significant factors: we also
  - Correlational Explanatory Faithfulness (CEF), need to test whether they mention significant
  - factors more often than insignificant ones.
  - bel distribution, more accurately reflecting the improves upon prior work by capturing both
  - the Llama2 family on three NLP tasks. We find Test (CCT), where we instantiate CEF on the
  - method, and identify ways to improve it.
  - searched in the literature, which we refer to as We identify two significant drawbacks with the CT:

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tle information about model predictions (Adebayo for an explanation to be 鈥渇aithful鈥. Jacovi and et al., 2018). It is therefore important to clearly Goldberg (2020) survey literature on the term and assess the extent to which explanations inform us define an explanation as faithful insofar as it 鈥渁c- about ML systems, both for current high-stakes curately represents the reasoning process behind applications such as medicine and criminal justice the model鈥檚 prediction鈥. Wiegreffe and Maraso- (Rudin, 2018), as well as potential scenarios involv- vic虂 (2021) review datasets for explainable NLP ing highly general systems (Shah et al., 2022; Ngo and identify three predominant classes of textual 530 Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 2: Short Papers), pages 530鈥546 August 11-16, 2024 漏2024 Association for Computational Linguistics explanations: highlights (also called extractive ex- cable to both approaches, we primarily focus on p
- **局限/未来工作线索**：To quantify the degree of prediction impact in a ple application of our method. Future work could；the model鈥檚 prediction. planations. Future work could apply the CCT to；Limitations jeet Agrawal, Dinesh Khandelwal, Parag Singla, and
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“The Probabilities Also Matter: A More Faithful Metric for Faithfulness of Free-Text Explanations in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
