# 176. TofuEval: Evaluating Hallucinations of LLMs on Topic-Focused Dialogue Summarization

> 逐篇阅读记录：第 176 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Liyan Tang, Igor Shalyminov, Amy Wing‐mei Wong, Jon Burnsky, Jake W. Vincent, Yu’an Yang, Siffi Singh, Song Feng, et al.
- **发表 venue / date**：NAACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.naacl-long.251)
- **领域标签**：Analysis, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 15,715 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Liyan Tang, Igor Shalyminov, Amy Wong, Jon Burnsky, Jake Vincent, Yu’an Yang, Siffi Singh, Song Feng, Hwanjun Song, Hang Su, Lijia Sun, Yi Zhang, Saab Mansour, Kathleen McKeown.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：5 LLMs；1 Introduction；2 Related Work；5 Results: LLMs as Evaluators；2021. MediaSum: A large-scale media interview 33B18 , we use the one based on Llama (Touvron；1 Pn provided individualized feedback to each annota-
- **引言关键线索**：capable of generating summaries of news articles Recently, the field of automated text summariza- that align with human preferences (Goyal et al., tion has been increasingly inclined toward using 2022; Zhang et al., 2023), we ask: can LLMs gen- Large Language Models (LLMs) to evaluate model erate factually consistent summaries without outputs (Fu et al., 2023; Gao et al., 2023; Madaan hallucinations for non-news domains? Given et al., 2023). Given the consequential nature of this the potential benefits that dialogue summarization trend, we ask: are LLMs up to the task of evalu- could bring to other areas, such as enhancing pro- ating model outputs? While recent work on news ductivity in meetings or streamlining customer ser- summarization has shown that LLMs鈥 performance vice interactions, we focus on dialogue summariza- at evaluating the factual consistency of generated tion as a case s
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Evaluator Selection For a comprehensive com- the imbalance of factually consistent and factually parison, we include the following non-LLM based inconsistent summaries. We analyzed the results SOTA factuality metrics: SummaC-ZS, SummaC- based on both sentence-level and summary-level CV (Laban et al., 2022), QAFactEval (Fabbri et al., prediction performance. Unless stated otherwise, 2022), and AlignScore (Zha et al., 2023). We also all evaluation results shown here are based on the include the following proprietary and open-source test set. LLMs as factual consistency evaluators: (1) GPT-4 (OpenAI, 2023); (2) GPT-3.5-Turbo; (3) Vicuna- Obtaining Predictions from non-LLM based 13B/33B (Chiang et al., 2023); (4) WizardLM- Factuality Metrics The non-LLM-based models 13B/30B (Xu et al., 2023). For all LLMs in the we used take as input a source and a piece of sum- aforementioned list we used a zero-shot configu- mary text to be evaluated, and
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 1, Comparison between T OFU, VAL and existing factual, Baseline 50.0 50.0 50.0, LLMs are the average, Note that a baseline, Al-, Further, BAcc, LLMs, Next, For example, FN tates a manual
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Table 1: Comparison between T OFU E VAL and existing factual consistency evaluation benchmarks on text
  - superior outcomes in comparison to zero-shot sce- using the development set and report the test set re-
  - - Baseline 50.0 50.0 50.0 50.0 50.0 50.0 50.0 50.0
  - for LLMs are the average of three runs. Note that a baseline method that always predicts inconsistent or consistent
  - dictions from the outputs of this prompt for model 60%, which is barely better than the baseline. Al-
  - and topic types most of the time. Further, most ingBank data, and it is even worse than the baseline.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：3 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - cinate significant amounts of factual errors in Figure 1: T OFU E VAL contains 1.5K topic-focused sum-
  - they perform poorly and can be outperformed mary, along with explanations and error types for factu-
  - rated error taxonomy. We find that there are Wang et al., 2023), they may not perform as well
  - we show that LLMs perform poorly on factual con-
  - to enable further research into improved automated
  - In text summarization, there have been significant hallucinations interchangeably in this work.
  - FU E VAL. We find that non-LLM-based metrics can infrastructure planning, and crime prevention.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Given the length of the dialogues in T OFU E VAL, we verse range subjects central to local governance and restrict the number of topics to three for each document. 4457 we prompt the LLM to generate main topics, our hu- that is being discussed or presented in the docu- man evaluation (more details in Section 3.4) shows ment. Marginal topics are those that are explored that while the majority of LLM-generated topics less thoroughly in the documents. More detailed are closely relevant, a small proportion of our col- definitions can be found in Appendix F.1. Main lected topics are marginally within the context of topics make up approximately 75% of the topics in the document. We decided to retain these marginal T OFU E VAL according to our categorization results topics based on the assumption that marginal topics (Table 5). can also be useful to summary readers. Factual Consistency A summary sentence is fac- 3.3 Summarization Model Selection tually consistent with a document if the senten
- **局限/未来工作线索**：fairly well at identifying what we termed Extrinsic Limitations；type of error primarily involves unfamiliar noun Our work has a few limitations. First, in our pro-；leave this for future work. have. Thirdly, our summarization evaluation is；leave this investigation to future work. Finally, we
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“TofuEval: Evaluating Hallucinations of LLMs on Topic-Focused Dialogue Summarization”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
