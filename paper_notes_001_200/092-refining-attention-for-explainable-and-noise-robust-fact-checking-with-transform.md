# 092. Refining Attention for Explainable and Noise-Robust Fact-Checking with Transformers

> 逐篇阅读记录：第 92 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jean-Flavien Bussotti, Paolo Papotti
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.1295)
- **领域标签**：Evaluation, Transformer, Behavior, Detect
- **本地 PDF 文本规模**：约 9,213 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：In tasks like question answering and factchecking, models must discern relevant information from extensive corpora in an "openbook" setting.Conventional transformer-based models excel at classifying input data, but (i) o
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction puts, which makes it challenging to discern the；2023. Enabling large language models to generate dar Joshi, Danqi Chen, Omer Levy, Mike Lewis,；2024. Faithful attention explainer: Verbalizing de- Elena Voita, David Talbot, Fedor Moiseev, Rico Sen-
- **引言关键线索**：specific evidence e虃 used for decision-making (Bus- Transformer-based models are pivotal in numerous sotti et al., 2024). Current post-hoc methods, like applications, notably in tasks like fact-checking, SHAP and LIME (Ribeiro et al., 2016; Lundberg which has gained prominence with the rise of so- and Lee, 2017), offer insights into model outputs, cial media platforms (Nakov et al., 2021). In this but are computationally expensive, requiring nu- domain, a claim, denoted as a query q, is examined merous executions. External models attempt to for its truthfulness using supporting evidence e虃 ex- classify context relevance (Atanasova et al., 2020), tracted from a vast corpus. The model processes but often fall short in faithfully representing the this query-evidence pair to produce an output o, model鈥檚 internal decision-making. Using LLMs for categorically labeling the claim as Supports, Re
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：module that directly modifies attention weights, context e are truly beneficial for decision-making, allowing the model both to improve predictions effectively aiming to expose the e虃 used by the and to identify the most relevant sections of in- model. Human fact-checkers dismiss outputs that put data after supervised training. We validate lack transparency about the evidence supporting our methodology using fact-checking datasets the model鈥檚 decision (Nakov et al., 2021). Ensur- and show promising results in question answer- ing transparency is vital for users, allowing them to ing. Experiments demonstrate improvements of up to 51% in F1 score for detecting relevant verify the evidence and determine their alignment context, and gains of up to 18% in task accuracy with the model鈥檚 conclusions (Guo et al., 2022). when integrating ATTUN into a model.1 However, traditional models often function as black boxes, lacking explanations for thei
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Transformer, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We then explain the, We compare ATTUN against, Table 1, Performance comparison across different, Verifier, Each evidence matrix is, T5 serves as the, Table 2, Comparison of models based, Table 4, Table 5, Table 6, Comparison of model performance, Question Answering on MS, Marco with and without
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tention between the context and the query within comparison to our approach. We then explain the
  - average attention weight between the context in- We compare ATTUN against models with intrinsic
  - Table 1: Performance comparison across different models and datasets for claim verification (Verifier) and explana-
  - several variants: to provide a comprehensive comparison of all pa-
  - manner for the claim span. Each evidence matrix is T5 serves as the baseline in this setting, while
  - Table 2: Comparison of models based on various characteristics.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - lack explainability regarding their decision pro- evant evidence would improve both accuracy and
  - allowing the model both to improve predictions effectively aiming to expose the e虃 used by the
  - ing. Experiments demonstrate improvements
  - context, and gains of up to 18% in task accuracy with the model鈥檚 conclusions (Guo et al., 2022).
  - during classification tasks, which significantly lim-
  - 2021). This underscores the need for improved
  - Finally, a significant challenge is the models鈥 2020) and scoring token/head salience (Liu et al.,

## 6. Conclusion、局限与可复现性

- **结论段落线索**：when integrating ATTUN into a model.1 However, traditional models often function as black boxes, lacking explanations for their out-
- **局限/未来工作线索**：categorically labeling the claim as Supports, Re- both output and justification has shown limitations；derscoring the value of shared learning in ATTUN. 5 Conclusion and Future Work；We observe that ATTUN brings the most bene- in input lengths. Future work could explore the；Limitations ases present in their training data, leading to unfair
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Refining Attention for Explainable and Noise-Robust Fact-Checking with Transformers”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
