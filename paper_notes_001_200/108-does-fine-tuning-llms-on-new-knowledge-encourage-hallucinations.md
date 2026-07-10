# 108. Does Fine-Tuning LLMs on New Knowledge Encourage Hallucinations?

> 逐篇阅读记录：第 108 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Zorik Gekhman, Gal Yona, Roee Aharoni, Matan Eyal, Amir Feder, Roi Reichart, Jonathan Herzig
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.444)
- **领域标签**：Analysis, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 13,671 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：When large language models are aligned via supervised fine-tuning, they may encounter new factual information that was not acquired through pre-training.It is often conjectured that this can teach the model the behavior
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction counter new factual information, extending beyond；6 SliCK Knowledge Categories Analysis less), which is a strong indicator that most of the；8 Discussion；9 Related Work 2024), probing internal representations (Azaria and；10 Conclusion；I Out-of-distribution (OOD) Evaluation accuracy for each of them for all the models of
- **引言关键线索**：the knowledge it acquired during pre-training. This Pre-training Large Language Models (LLMs) on raises the question of how LLMs integrate new textual corpora embeds substantial factual knowl- facts outside of their pre-existing knowledge. One edge in their parameters (Petroni et al., 2019; possibility is that the model simply adapts by learn- AlKhamissi et al., 2022; Cohen et al., 2023), which ing this new factual information. However, a com- is essential for excelling in various downstream mon conjecture posits that such exposure to new applications. These models often require further knowledge may encourage the model to halluci- alignment to desired behaviors, typically achieved nate factually incorrect responses, as the model through supervised fine-tuning on instruction- is essentially trained to generate facts that are not following tasks (Wei et al., 2022; Mishra et al., grounded 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：November 12-16, 2024 漏2024 Association for Computational Linguistics 2023; Gudibande et al., 2023). Figure 1). Alternatively, we also show that filtering- In this work, we study how learning new factual out the Unknown fine-tuning examples substantially knowledge through fine-tuning impacts the model鈥檚 reduces the risk of overfitting, without sacrificing tendency to hallucinate w.r.t. its pre-existing knowl- performance. edge, exploring the above conjecture.1 We further evaluate the impact of fine-tuning To study the impact of new knowledge, we must examples from each of our three Known knowl- be able to assess whether a single fine-tuning ex- edge categories on performance (搂5). Unexpect- ample is consistent with the model鈥檚 knowledge. edly, we find that a model fine-tuned only on exam- We propose SliCK, a hierarchy of four knowl- ples from the highest knowledge degree, denoted edge categories, derived from a continuous mea- HighlyKnow
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Assessing a model, As a case study, See, More details in, This each column. To, Figure 3a, Fig- Since we also, OOD test performance for, EARLY, STOP to C ONVERGENCE, Figure 5 we compare, Unknown category vs
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Assessing a model鈥檚 knowledge remains an open As a case study for comparison, we analyze the
  - See 搂J for details about this statistic significance test. sive comparison for future work. More details in 搂K.
  - that M fits during different fine-tuning stages. This each column. To this end we compare all the values
  - results (Figure 3a) were discussed in 搂4.1. Fig- Since we also discuss 鈥渉orizontal鈥 comparisons,
  - ure 8b presents the OOD test performance for the where we compare EARLY _ STOP to C ONVERGENCE,
  - et al. (2022) as a case study for comparison. In

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：6 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - learned significantly slower than those consis-
  - cinate. Taken together, our results highlight
  - ample is consistent with the model鈥檚 knowledge. edly, we find that a model fine-tuned only on exam-
  - to the model. The Known examples are subse- significantly influences the extent to which LLMs
  - design a controlled study, focused on closed-book controlled setup that isolates this factor. We find
  - Unknown. In Figure 4, we show that this behav-
  - significantly slow fitting rate suggests that LLMs

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Known, we put emphasis on greedy decoding out- comes, represented with PCorrect (q, a; M, T = 0). 4.1 Higher Unknown Ratio is Proportional to HighlyKnown represents (q, a) pairs for which M Performance Degradation always greedily predicts a. If M sometimes (but Figure 3a presents the performance as a function not always) greedily predicts a, we consider (q, a) of the % of Unknown examples in D, for different as MaybeKnown. Lastly, if M never greedily pre- fine-tuning durations. Higher %Unknown leads to dicts a, we classify (q, a) as WeaklyKnown. performance degradation, regardless of the fine- We apply SliCK to annotate each (q, a) pair in tuning duration, which indicates that Unknown our dataset with its knowledge category w.r.t. M .8 examples are less useful than Known. Perfor- We analyze the quality of our categories in 搂6. mance is also strongly affected by the fine-tuning duration, with EARLY _ STOP typically yielding the
- **局限/未来工作线索**：which we leave for future work. triplets, which are converted into closed-book QA；See 搂J for details about this statistic significance test. sive comparison for future work. More details in 搂K.；relatively short fine-tuning runs, and testing only 11 Limitations；One limitation of using the Unknown examples
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Does Fine-Tuning LLMs on New Knowledge Encourage Hallucinations?”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
