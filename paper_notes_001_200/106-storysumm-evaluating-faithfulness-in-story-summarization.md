# 106. STORYSUMM: Evaluating Faithfulness in Story Summarization

> 逐篇阅读记录：第 106 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Melanie Subbiah, Faisal Ladhak, Akankshya Mishra, Griffin Adams, Lydia B. Chilton, Kathleen McKeown
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.557)
- **领域标签**：Rationale, Evaluation, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 12,609 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Human evaluation has been the gold standard for checking faithfulness in abstractive summarization.However, with a challenging source domain like narrative, multiple annotators can agree a summary is faithful, while miss
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction strategies generate questions about the summary；4 Benchmarking Automatic Metrics remaining methods, we show results on the full；7 Limitations Josh Achiam, Steven Adler, Sandhini Agarwal, Lama；2023. Longeval: Guidelines for human evaluation Chenglei Si, Navita Goyal, Sherry Tongshuang Wu,；2023. Alignscore: Evaluating factual consistency an expression of love, and the story ends by saying he will
- **引言关键线索**：and compare answers retrieved from the summary As Large Language Models (LLMs) are able to vs. the source document (Durmus et al., 2020; Fab- perform more open generation tasks, challenges bri et al., 2021b). Entailment-based approaches in evaluation have arisen (Gabriel et al., 2020). align facts in the summary with evidence from the Summarization is one such task. Some aspects source and determine for each pair if the evidence of summary quality like readability or coherence entails the fact (Utama et al., 2022; Laban et al., (Goyal et al., 2022; Chang et al., 2023) can be 2022; Maynez et al., 2020). More recent work ex- judged by looking at the summary alone. How- plores prompting strategies for LLMs to identify ever, judging faithfulness (whether all details in the faithfulness errors (Min et al., 2023; Kim et al., summary are faithful to the source) requires care- 2024a; Si et al., 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：method can detect challenging inconsistencies. Using this dataset, we first show that any one human annotation protocol is likely to miss in- consistencies, and we advocate for pursuing Figure 1: A S TORY S UMM example illustrating an in- a range of methods when establishing ground correct interpretation of double entendre. A standard truth for a summarization dataset. We finally fine-grained human annotation protocol missed this in- test recent automatic metrics and find that none consistency even though it is obvious once pointed out. of them achieve more than 70% balanced ac- curacy on this task, demonstrating that it is a challenging benchmark for future work in faith- 2) human crowdworkers. Model-based approaches fulness evaluation. typically build on QA or entailment strategies. QA
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Upwork annotator labels each, We hypothesize that identify-, LLM raises the chances, Expert, Three of the authors, Label Comparison, Table 6
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - fore, we compare our Upwork annotator labels each inconsistency. We hypothesize that identify-
  - addition to our annotator labels, we compare the possible options from the LLM raises the chances
  - Expert. Three of the authors of this paper review 3.1 Label Comparison
  - Table 6: A comparison of the summary statistics be-
  - a possible inconsistency - do not just by comparison of course, I am not satisfied merely with the

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - improve evaluation methods for faithfulness. S TO -
  - to push our methods forward. the purpose of this dataset is to improve detection
  - We first explore how to establish ground-truth on of errors in LLM-generated summaries. We show
  - ing task more consistently. We find that different We collect a dataset of 32 short stories from two
  - marization. Most importantly, we show that it is maries for their stories, and since these stories are
  - tomatic metrics perform on this dataset. We find are now being used to summarize lots of differ-
  - tors in a group label it as such. We find that MTurk ods, and only apply to unfaithful summaries. We

## 6. Conclusion、局限与可复现性

- **结论段落线索**：not be re-shared under another name. Finally, we We introduce a new benchmark for testing methods release the dataset without user-identifying infor- for faithfulness evaluation. In producing the bench- mation to protect user privacy. One of the authors, mark, we demonstrate that faithfulness in narra- Melanie Subbiah, has an equity interest in OpenAI. tive summarization is still a significant concern for LLMs, and we formulate recommendations for bet- Acknowledgements ter evaluation of faithfulness in summaries. Finally, We would like to express our gratitude to the Up- we demonstrate that recent automatic evaluation work and MTurk workers for contributing annota- metrics have room for improvement on this task. In tions for this work. Additionally, we would like the future, we hope to use this dataset to improve to thank our reviewers for their thoughtful feed- methods for reliable evaluation of narrative sum- back. This work was made possible by the gener- marization. In particular, 
- **局限/未来工作线索**：challenging benchmark for future work in faith- 2) human crowdworkers. Model-based approaches；caution future work to avoid using MTurk for faith- tive summarization with close to 40% of summaries；We recommend future work use the expanded gold against the Upwork annotator labels in Table 7 to；7 Limitations Josh Achiam, Steven Adler, Sandhini Agarwal, Lama
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“STORYSUMM: Evaluating Faithfulness in Story Summarization”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
