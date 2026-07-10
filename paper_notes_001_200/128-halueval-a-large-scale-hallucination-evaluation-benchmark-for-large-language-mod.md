# 128. HaluEval: A Large-Scale Hallucination Evaluation Benchmark for Large Language Models

> 逐篇阅读记录：第 128 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Junyi Li, Xiaoxue Cheng, Xin Zhao, Jian‐Yun Nie, Ji-Rong Wen
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.397)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 9,600 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Large language models (LLMs), such as ChatGPT, are prone to generate hallucinations, i.e., content that conflicts with the source or cannot be verified by the factual knowledge.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction the Hallucination Evaluation benchmark for Large；60. Morehead is a home rule-class city located along US 60 (the historic Midland Trail) and Interstate 64 in；4 Related Work；1. July 4, 1776 - Declaration of Independence sign- As an AI language model, I cannot create visualizations,；2. April 12, 1861 - Start of the Civil War According to World Bank data, India and China have；3. August 18, 1920 - 19th Amendment ratification gr- shown significant economic growth in the past two de-；4. December 7, 1941 - Pearl Harbor attack during Wo- In the following years, the GDP growth rate fluctuat-；1. August 2, 1776 - Declaration of Independence si-
- **引言关键线索**：Language Models (HaluEval): a large collection The advent of large language models (LLMs) (Zhao of 35,000 hallucinated/normal samples for LLMs et al., 2023) has ushered in a paradigm shift in nat- analysis and evaluation. HaluEval includes 5,000 ural language processing (NLP), making unprece- general user queries with ChatGPT responses and dented progress in text generation and understand- 30,000 task-specific examples from three tasks, i.e., ing (Brown et al., 2020; Li et al., 2021). The re- question answering, knowledge-grounded dialogue, markable language ability makes LLMs core in a and text summarization. The construction pipeline number of products with millions of users, such as of HaluEval is depicted in Figure 1. For general the coding assistant Copilot and recent ChatGPT. user queries, we adopt the 52K instruction tuning Despite these prominent capabilities of LLMs dataset from
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：lijunyi@ruc.edu.cn chengxiaoxue3@gmail.com batmanfly@gmail.com Abstract User Query Retrieve the oldest photo of a cat ChatGPT There is no definitive answer to this question Large language models (LLMs), such as Chat- as 鈥渢he oldest photo of a cat鈥 is subjective and varies based GPT, are prone to generate hallucinations, i.e., on different sources and definitions. However, one can- content that conflicts with the source or cannot didate for the oldest cat photo is the daguerreotype be verified by the factual knowledge. To un- 鈥淐at with Blue Ribbon,鈥 taken by photographer Antoine derstand what types of content and to which ex- Claudet in 1840. The photo depicts a cat sitting on a table, tent LLMs are apt to hallucinate, we introduce gazing directly at the camera, and wearing a blue ribbon tied around its neck. [...] the Hallucination Evaluation benchmark for Large Language Models (HaluEval), a large Table 1: An example from Alpaca (Taori 
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：未自动提取到稳定短句，需回看论文中的 Baselines、Comparison 或 Ablation 小节。

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - responding spans. As shown in Table 1, for the 鈥 Second, existing LLMs face significant chal-
  - conceive effective methods to alleviate it. recognizing hallucinations can be improved by pro-
  - of QA, dialogue, and summarization. We show
  - recognition experiments, then propose several po- We find that the hallucination of LLMs is topic-
  - tentially useful strategies to improve the recogni- sensitive. For example, the frequent topics in QA
  - 3.2.2 Improvement Strategies
  - In this part, we design several strategies to improve evaluation for LLMs.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：focusing on evaluating the hallucination of models in different NLP tasks (Dziri et al., 2022b; Gupta We introduce HaluEval, a large-scale collection of et al., 2022; Dziri et al., 2022a; Rashkin et al., 2021; generated and human-annotated hallucinated sam- Li et al., 2023b). For instance, The BEGIN bench- ples for evaluating the performance of LLMs in mark (Dziri et al., 2022b) classifies the utterances recognizing hallucinations. To automatically gen- generated by dialogue systems into three categories, erate large-scale samples, we propose a two-step 6456 approach, i.e., sampling-then-filtering. We first in- References troduce two different sampling methods to generate 2023. Introducing Falcon LLM . https://falconllm. diverse samples using instructions and then filter tii.ae. and select the difficult one. Besides, we invite qual- ified human labelers to annotate the hallucinations Yejin Bang, Samuel Cahyawijaya, Nayeon Lee, Wen- of ChatGPT responses given user queries. We find liang
- **局限/未来工作线索**：6 Limitations learners. arXiv preprint arXiv:2005.14165.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“HaluEval: A Large-Scale Hallucination Evaluation Benchmark for Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
