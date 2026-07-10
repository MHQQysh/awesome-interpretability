# 145. Enabling Large Language Models to Generate Text with Citations

> 逐篇阅读记录：第 145 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Tianyu Gao, H. W. Yen, Jiatong Yu, Danqi Chen
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.398)
- **领域标签**：Evaluation, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 17,935 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Large language models (LLMs) have emerged as a widely-used tool for information seeking, but their generated outputs are prone to hallucination.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction closed-source models, making their results difficult；4 Modeling；5 Experiments；60 Oracle；40 Oracle 20 ChatGPT (VANILLA)；6 Human Evaluation；7 Related Work rectness, and citation quality, and verify their effi-；2022. Training language models to follow instruc-
- **引言关键线索**：to evaluate. Retrieval-augmented LMs (Borgeaud Large language models (LLMs; Brown et al., 2020; et al., 2022; Izacard et al., 2022) incorporate re- OpenAI, 2023) have gained increasing popularity trieved passages during both training and infer- as a tool for information seeking. While they gener- ence, but do not guarantee faithfulness to retrieved ate engaging and coherent responses, their outputs passages or explicitly provide citations. Addition- are prone to hallucination and often contain fac- ally, previous studies mostly rely on human eval- tually incorrect information (Ji et al., 2023). This uation (Nakano et al., 2021; Menick et al., 2022; makes it harder for users to trust and verify LLM- Liu et al., 2023), which is expensive and difficult to generated outputs without any supporting evidence. reproduce. We argue that the absence of automated In this work, we study a new generat
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：first benchmark for Automatic LLMs鈥 Citation the system generates text while providing citing pas- Evaluation. ALCE collects a diverse set of sages from a large retrieval corpus. Each statement may questions and retrieval corpora and requires contain multiple citations (e.g., [1][2]). building end-to-end systems to retrieve sup- porting evidence and generate answers with citations. We develop automatic metrics along to provide citations to one or a few text passages three dimensions鈥攆luency, correctness, and ci- for any statement they generate (Figure 1). Incor- tation quality鈥攁nd demonstrate their strong porating citations brings several benefits: (1) users correlation with human judgements. Our exper- can easily verify LLMs鈥 claims with the provided iments with state-of-the-art LLMs and novel citations; (2) LLMs can generate text that faithfully prompting strategies show that current systems follows cited passages, which has the promi
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：ChatGPT and GPT-4 baselines, We use short instructions, LLaMA, Comparison of Different LLMs, Table 7, Comparison of different LLMs, ASQA, We compare the impact, Table 10, Comparison between ROUGE-L and, Detailed retrieval results. We, ALCE assumes, Effect of Instructions FiD
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - of our ChatGPT and GPT-4 baselines are not fully
  - book baseline, where the model is only prompted text window. We use short instructions for LLaMA
  - 5.2 Comparison of Different LLMs
  - serves as an upper bound for model performance, and we compare them with two models鈥 correctness results in the
  - Table 7: Comparison of different LLMs on ASQA
  - sages (our primary baseline); (2) an oracle ver- question; (2) citation recall: the annotator is given

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：66.1%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - have considerable room for improvement鈥擣or
  - example, on the ELI5 dataset, even the best improve correctness and alleviate hallucination.
  - quires building end-to-end systems to retrieve rel- to retrieve, they do not improve the performance
  - sponse to the question, and cite corresponding sup- the retrieved passages in a shorter text improves
  - shown in Table 1. Different from previous bench- GPT but improves GPT-4 performance.
  - natural language inference (NLI) model (Honovich brings significant improvement. These challenges
  - et al., 2022) to measure citation quality. We show- pose promising research directions for developing

## 6. Conclusion、局限与可复现性

- **结论段落线索**：automatic evaluation achieves high accuracy when treating human annotations as gold labels (85.1% We propose ALCE, the first automatic benchmark for citation recall and 77.6% for citation precision). for evaluating LLM generations with citations. We deploy automatic metrics to measure fluency, cor-
- **局限/未来工作线索**：(a) 蠒(ci,j , si ) = 0, AND To tackle this limitation, we propose to provide；Limitations Lespiau, Bogdan Damoc, Aidan Clark, Diego；LLMs to generate citations for future work. Angela Fan, Yacine Jernite, Ethan Perez, David Grang-；averaged scores for all experiments in the main yields poor results. We defer it to future work to
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Enabling Large Language Models to Generate Text with Citations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
