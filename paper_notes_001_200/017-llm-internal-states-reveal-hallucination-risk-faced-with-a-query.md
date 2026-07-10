# 017. LLM Internal States Reveal Hallucination Risk Faced With a Query

> 逐篇阅读记录：第 17 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ziwei Ji, Delong Chen, Etsuko Ishii, Samuel Cahyawijaya, Yejin Bang, Bryan Wilie, Pascale Fung
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.blackboxnlp-1.6)
- **领域标签**：Probing, Evaluation, Hallucination, Hidden, Layer, Detect
- **本地 PDF 文本规模**：约 11,026 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：The hallucination problem of Large Language Models (LLMs) significantly limits their reliability and trustworthiness.Humans have a selfawareness process that allows us to recognize what we don't know when faced with quer
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Hallucination and Training Data probing classifier technique (Belinkov, 2022) on；8 Limitation Acknowledgement；13 Task 574: air dialogue sentence generation, Task 361: spolin yesand prompt response classification,；2 Task 1703: ljspeech textmodification, Task 1704: ljspeech textmodification；2 Task 039: qasc find overlapping words, Task 281: points of correspondence；90 Task 113: count frequency of letter, Task 1151: swap max min, Task 509: collate of all alphabetical and；207 Task 837: viquiquad answer generation, Task 701: mmmlu answer generation high school computer；1 Task 1340: msr text compression compression
- **引言关键线索**：2024) have explored the internal states of language Humans have an awareness of the scope and limit models that capture contextual and semantic infor- of their own knowledge (Fleming and Dolan, 2012; mation learned from training data (Liu et al., 2023; Koriat, 1997; Hart, 1965), as illustrated in Fig. 1. Chen et al., 2024; Gurnee and Tegmark, 2023). This cognitive self-awareness ability in humans Nevertheless, internal states of language models introduces hesitation in us before we respond sometimes exhibit limited generalization on unseen to queries or make decisions in scenarios where data and their representation effectiveness can be we know we don鈥檛 know (Yeung and Summer- undermined by flawed training data or modeling is- field, 2012; Nelson, 1990; Bland and Schaefer, sues (Wang et al., 2022a; Belinkov and Glass, 2019; 2012). However, LLM-based AI assistants lack this Meng et al., 2
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：query in training data, achieving an accuracy of This section begins with an introduction to the prob- 80.28%. (2) Whether they are likely to hallucinate lem formulation of uncertainty estimation faced regarding the query, achieving an average estima- with queries in 搂 3.1. We construct datasets in 搂 3.2 tion accuracy of 84.32% across 15 NLG tasks. We focusing on two dimensions: (1) the distinction propose that understanding these representations between queries seen and unseen in the training could offer a proactive approach to estimating un- data; (2) the likelihood of hallucination risk faced certainty, potentially serving as an early indicator with the queries. To validate the efficacy of internal for the necessity of retrieval augmentation (Wang state representation in hallucination estimation, we et al., 2023) or as an early warning system. visualize the neurons for perception extracted from a specified LLM layer (搂 3.3) and then 
- **方法与解释性关系**：该论文主要围绕 `Probing, Evaluation, Hallucination, Hidden, Layer, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Perplexity, PPL, Zero-shot, The former focuses on, INSIDE, Chen et al, PPL-based and prompt-based baselines
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - are adept at recognizing complex patterns and re- and baselines including Perplexity (PPL), Zero-shot
  - better than the baselines in the estimation task, it tion for questions. The former focuses on whether
  - ments. INSIDE (Chen et al., 2024) also leverages PPL-based and prompt-based baselines, with an

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：80.28%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Models (LLMs) significantly limits their relia-
  - Fig. 6, the hallucination rate fluctuates significantly
  - lation task gain higher gradients and significantly
  - et al., 2019; Yang et al., 2021; Zheng et al., 2020) 7 Conclusion
  - internal state can reveal the truthfulness of state- internal state-based self-assessment outperforms
  - robustness and applicability of our results. References
  - Llama MLP outperforms the standard MLP.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：are also relevant areas dealing with the differenti- ation of unknown/unseen from training data, with Inspired by human self-awareness, this work main focus on classification tasks. Our method is demonstrates the latent capacity of LLMs to self- versatile across various NLG tasks without requir- assess and estimate hallucination risks prior to re- ing fine-tuning of LLMs. sponse generation. We conduct a comprehensive analysis of the internal states of LLMs both in terms of training data sources and across 15 NLG tasks Hallucination Detection The phenomenon of with over 700 datasets. Employing a probing es- hallucination in NLG encourages a variety of de- timator on the internal states associated with the tection methods (Min et al., 2023; Ji et al., 2024; Li queries, we assess their self-awareness and ability et al., 2023; Scialom et al., 2021). Some of these to indicate uncertainty in two aspects: (1) recog- methods delve into the internal states for detection. nizing whether they hav
- **局限/未来工作线索**：For future work, we aim to refine our methodol-；8 Limitation Acknowledgement
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“LLM Internal States Reveal Hallucination Risk Faced With a Query”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
