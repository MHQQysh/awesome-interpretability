# 126. A Mechanistic Interpretation of Arithmetic Reasoning in Language Models using Causal Mediation Analysis

> 逐篇阅读记录：第 126 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Alessandro Stolfo, Yonatan Belinkov, Mrinmaya Sachan
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.435)
- **领域标签**：Mechanistic, Reasoning, Hallucination, Layer, Analyze
- **本地 PDF 文本规模**：约 10,907 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Mechanistic为主线，结合论文摘要中的核心设定：Mathematical reasoning in large language models (LMs) has garnered significant attention in recent work, but there is a limited understanding of how these models process and store information related to arithmetic tasks
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Work；3 Methodology；231 Prediction: 121 Prediction: 231；1. Given fO , we sample two sets of operands；18. For the attention (Figures 3g and 3h), we do besides the early layers at the operand tokens, is the；6 Conclusion periments, we consider the output of an attention；100 Multiplication Division
- **引言关键线索**：or specific prompting techniques (Wei et al., 2022b; Mathematical reasoning with Transformer-based Kojima et al., 2022; Yang et al., 2023, inter alia). models (Vaswani et al., 2017) is challenging as However, there is a limited understanding of the it requires an understanding of the quantities and inner workings of these models and how they store the mathematical concepts involved. While large and process information to correctly perform math- language models (LMs) have recently achieved based tasks. Insights into the mechanics behind impressive performance on a set of math-based LMs鈥 reasoning are key to improvements such as tasks (Wei et al., 2022a; Chowdhery et al., 2022; inference-time correction of the model鈥檚 behavior OpenAI, 2023), their behavior has been shown (Li et al., 2023) and safer deployment. Therefore, to be inconsistent and context-dependent (Bubeck research in this dir
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：other tasks, including number retrieval from mance of large LMs on math benchmark datasets prompts and factual knowledge questions.1 through enhanced pre-training (Spokoyny et al., 2022; Lewkowycz et al., 2022; Liu and Low, 2023)
- **方法与解释性关系**：该论文主要围绕 `Mechanistic, Reasoning, Hallucination, Layer, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Finally, Stolfo et al, This comparison validates knowledge, Section 4.1. In particular
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - dynamics, we compare the effects of different
  - 6B, and 7B parameters. Finally, we compare the area, Stolfo et al. (2023) use a causally-grounded
  - to factual knowledge. This comparison validates knowledge, our study represents the first attempt
  - the two settings differ: the main contribution to Section 4.1. In particular, we compare the informa-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：15      times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - els (LMs) has garnered significant attention
  - their architecture. In order to improve our un-
  - model components on arithmetic queries with of works proposing methods to improve the perfor-
  - impressive performance on a set of math-based LMs鈥 reasoning are key to improvements such as
  - lead to the same conclusions. as larger integers get split into multiple tokens by the tokenizer.
  - (2). These observations point to the conclusion that 1st Operand
  - In Figures 3e鈥3h, we show a re-scaled version of as its smaller size allows for less computationally

## 6. Conclusion、局限与可复现性

- **结论段落线索**：7038 (a) (b) (e) (f) (g) (c) (d) (h) Figure 3: Indirect effect (IE) measured within GPT-J. Figures (a) and (b) illustrate the flow of information related to both the operands and the result of the queries, while the effect displayed in Figures (c) and (d) is related to the operands only (the result is kept unchanged). Figures (e鈥揾) show a re-scaled visualization of the effects at the last token for each of the four heatmaps (a鈥揹). The difference in the effect registered for the MLPs at layers 15鈥25 between figures (a) and (c) illustrates the role of these components in producing result-related information. Our analysis reveals four primary activation sites: mation within Transformer-based models to the the MLP module situated at the first layer corre- attention mechanism (Elhage et al., 2021; Geva sponding to the tokens of the two operands; the et al., 2023), while the feed-forward layers are as- intermediate attention blocks at the last token of sociated with performing computations, 
- **局限/未来工作线索**：into account the magnitude of the effect. Finally, a limitation of our work concerns the；Limitations Nora Belrose, Zach Furman, Logan Smith, Danny Ha-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“A Mechanistic Interpretation of Arithmetic Reasoning in Language Models using Causal Mediation Analysis”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
