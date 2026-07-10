# 060. The Eval4NLP 2023 Shared Task on Prompting Large Language Models as Explainable Metrics

> 逐篇阅读记录：第 60 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Christoph Leiter, Juri Opitz, Daniel Deutsch, Yang Gao, Rotem Dror, Steffen Eger
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.eval4nlp-1.10)
- **领域标签**：Rationale, Evaluation, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 12,921 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Generative large language models (LLMs) have seen many breakthroughs over the last year.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Work；3 Shared Task Setup unforeseen issues with extending the competition；5. This makes sense, as MQM weighs major er- more reliable human annotations than other anno-；5 Annotation MT We construct the MT dataset from random；7 Results and Analysis limited, leading many of them to opting for smaller；8 Conclusion its capability of translation, therefore favoring；2023. Reference-free summarization evaluation with Ma, Ahmed El-Kishky, Siddharth Goyal, Mandeep
- **引言关键线索**：acquiring training data (e.g. Xu et al., 2023b) and The ChatGPT revolution in late 2022 has ignited fine-tune models to specific tasks. Given this typi- a wide public and scientific debate about the pos- cal focus on fine-tuning and motivated by promis- sibilities (and limitations) of generative AI in var- ing work on prompting techniques2 (e.g. Wei et al., ious fields and application scenarios (Leiter et al., 2 Various websites track the development of prompt- 1 https://github.com/eval4nlp/SharedTask2023/tree/main ing techniques, e.g. https://www.promptingguide. 117 Proceedings of the 4th Workshop on Evaluation and Comparison of NLP Systems, pages 117鈥138 November 1, 2023 漏2023 Association for Computational Linguistics Figure 1: Using a generative LLM as MT evaluation metric. In this example, the metric is reference-free. I.e. it grades the translated sentence based on its source senten
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：been successfully employed as evaluation met- age generative large language models (LLMs) as rics in text generation tasks. Approaches often evaluation metrics (Kocmi and Federmann, 2023; differ in the input prompts, the samples that are Liu et al., 2023b; Fu et al., 2023; Xu et al., 2023b; selected for demonstration and the construc- Fernandes et al., 2023) for natural language genera- tion process of scores from the output. Within this context, we introduce the Eval4NLP 2023 tion (NLG) tasks like machine translation (MT) and shared task that asks participants to explore summarization. Recent LLM based approaches dif- such approaches for machine translation eval- fer, for example, in their prompting strategies, e.g., uation and summarization evaluation. Specifi- in the way that natural language instructions are cally, we select a list of allowed LLMs and dis- used to trigger the LLM to compute metric scores. allow fine-tuning to ensure
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Proceedings of the 4th, Workshop on Evaluation and, Comparison of NLP Systems, Pradhan and Todi, Baselines As baselines, On the other hand, LTRC, Further, Codabench would sometimes fail, For this subtask
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Proceedings of the 4th Workshop on Evaluation and Comparison of NLP Systems, pages 117鈥138
  - line approaches, we compare the participants with.
  - an output-based setting. Pradhan and Todi (2023) Baselines As baselines, we use the widely used
  - include one baseline for every allowed model that test-phase participants, 9 have submitted a system
  - participants might have optimized their method on the dev-set they beat the large model baselines that
  - On the other hand, we wanted to give participants ous baseline models and team LTRC.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：5 points, 124
                  X, 136
                   X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - cusses future work and provides a conclusion. and BARTScore (Yuan et al., 2021), generation-
  - brief overview of evaluation metrics, highlight the more high-performing LLMs has shown improved
  - created after 15.07.23 as source texts.13 new template (we show a screenshot of the UI in
  - (in a random order). This has majorly improved agreement on these subsets. For en-es, either the
  - (2021): relevance, factuality, and readability, where and different normalization approaches. We find
  - e.g., by concatenating facts in the wrong order. improve the source quality and ordered the sources
  - small atomic facts of many human written refer- further improve the fine-grained score composition but would

## 6. Conclusion、局限与可复现性

- **结论段落线索**：based metrics have shown strong performance.
- **局限/未来工作线索**：and analyze paths for future work. Finally, as a tributed to the exploration of prompting for NLG；sibilities (and limitations) of generative AI in var- ing work on prompting techniques2 (e.g. Wei et al.,；cusses future work and provides a conclusion. and BARTScore (Yuan et al., 2021), generation-；that are based on lexical overlap have limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“The Eval4NLP 2023 Shared Task on Prompting Large Language Models as Explainable Metrics”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
