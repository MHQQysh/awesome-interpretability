# 182. Few-Shot Self-Rationalization with Natural Language Prompts

> 逐篇阅读记录：第 182 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ana Marasović, Iz Beltagy, Doug Downey, Matthew E. Peters
- **发表 venue / date**：NAACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.findings-naacl.31)
- **领域标签**：Rationale, Evaluation, Transformer, Behavior, Explain
- **本地 PDF 文本规模**：约 10,014 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Self-rationalization models that predict task labels and generate free-text elaborations for their predictions could enable more intuitive interaction with NLP systems.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；3 Prompting for Self-Rationalization scribe these formats in detail and compare their；I NFILLING；6 Conclusions Clark, Christopher Berner, Sam McCandlish, Alec；2020. Natural language rationales with full-stack；2018. Record: Bridging the gap between human
- **引言关键线索**：be more informative in the fewshot setting. A few Models constrained to be more understandable to prompts are then used for finetuning. In this paper, people are easier to troubleshoot and more useful we explore whether prompt-based finetuning can in practice (Rudin et al., 2021). For instance, con- be extended to induce few-shot self-rationalization straining a model that answers the question 鈥淲hich behavior in addition to few-shot prediction. To linguist invented the lightbulb?鈥 with 鈥渘one鈥 to measure our progress, we first introduce FEB as a also provide the reason鈥斺淭homas Edison is the benchmark dataset consisting of human authored inventor of the lightbulb and he was not a linguist鈥濃 free-text explanations across four distinct end tasks makes the model easier to control and interact with including natural language inference and common- (Kim et al., 2021). Models that jointly predict
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：natural language prompts on FEB. Then, by us- munity begin tackling self-rationalization with only ing this prompt and scaling the model size, we a few examples, we present (i) FEB鈥攁 standard- demonstrate that making progress on few-shot ized collection of four existing English-language self-rationalization is possible. We show there datasets and associated metrics, and (ii) the first ap- is still ample room for improvement in this task: proach for the task established through an extensive the average plausibility of generated explana- tions assessed by human annotators is at most evaluation of natural language prompts.1 51% (with GPT-3), while plausibility of human One approach to few-shot learning is prompt- explanations is 76%. We hope that FEB and based finetuning with natural language prompts. our proposed approach will spur the commu- Such prompts are produced by formatting finetun- nity to take on the few-shot self-rationalizatio
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Transformer, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We format, T5 is one of, However, ANDOM BASELINE 20.0 -, ANDOM BASELINE 50.0 -, Finally, NI F EW 66.10.4, NIFIED QA has lost, Table 3, Table 4
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - plify future model comparison and lower barriers biases that are implied in language. We format
  - least slightly above the random baseline across all four tasks.
  - T5 is one of the largest open-sourced and widely strong baselines for classification tasks. However,
  - label choices is not beneficial and in some cases R ANDOM BASELINE 20.0 -
  - that this prompt is both versatile and effective. R ANDOM BASELINE 50.0 -
  - Finally, we compare the best performing U NI F EW 66.10.4 63.80.4

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：27.7 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - self-rationalization is possible. We show there datasets and associated metrics, and (ii) the first ap-
  - is still ample room for improvement in this task:
  - and end-task accuracy improve with increasing
  - results show that there is still a large gap between challenging. The last requirement is introduced to
  - types with FEB. Our results show that a unified tive QA models, we investigate two models: T5
  - and tags in the input outperforms U NI F EW for both
  - C OM VE, we observe notable improvements from

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Radford, Ilya Sutskever, and Dario Amodei. 2020. We draw attention to the task of few-shot self- Language models are few-shot learners. In Ad- rationalization: predicting task labels and gener- vances in Neural Information Processing Systems, ating free-text explanations for the prediction using volume 33, pages 1877鈥1901. Curran Associates, only a few human-written explanations. We present Inc. (i) the FEB benchmark, (ii) the first prompting ap- Oana-Maria Camburu, Tim Rockt盲schel, Thomas proach for FEB established through a comprehen- Lukasiewicz, and Phil Blunsom. 2018. e-SNLI: Nat- sive search of natural language prompts, and (iii) ural language inference with natural language expla- 418 nations. In Proceedings of the Advances in Neural Dan Hendrycks, Collin Burns, Steven Basart, Andy Information Processing Systems (NeurIPS). Zou, Mantas Mazeika, Dawn Song, and Jacob Stein- hardt. 2021. Measuring massive multitask language Samuel Carton, Anirudh Rathore, and Chenhao Tan. understand
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Few-Shot Self-Rationalization with Natural Language Prompts”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
