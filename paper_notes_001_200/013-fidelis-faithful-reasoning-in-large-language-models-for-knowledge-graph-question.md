# 013. FiDeLiS: Faithful Reasoning in Large Language Models for Knowledge Graph Question Answering

> 逐篇阅读记录：第 13 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yuan Sui, Yufei He, Nian Liu, X. He, Kun Wang, Bryan Hooi
- **发表 venue / date**：ACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-acl.436)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Evaluate
- **本地 PDF 文本规模**：约 10,471 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Large Language Models (LLMs) are often challenged by generating erroneous or hallucinated responses, especially in complex reasoning tasks.Leveraging Knowledge Graphs (KGs) as external knowledge sources has emerged as a
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2014 World Series Retrieval Reasoning Step；3 Method user question, ensuring the final reasoning path is；4 Experiments；6 Conclusion；5 Related Work；140 WebQSP CWQ CR-LT
- **引言关键线索**：Large Language Models (LLMs) have shown im- Existing KG-enhanced LLM reasoning methods pressive reasoning capabilities in tackling complex face notable challenges and can be roughly cate- tasks (Yu et al., 2024; Sui et al., 2025). However, gorized into two primary approaches: retrieval- their reasoning processes are not always reliable, based and agent-based paradigms (Luo et al., and can be prone to generating outputs that are ei- 2025). Retrieval-based methods (Wang et al., 2023; ther inconsistent with real-world facts (Xu et al., Luo et al., 2024; Baek et al., 2023) retrieve rel- 2024; Huang et al., 2025) or show flawed reason- evant KG facts to support LLM reasoning by ei- ing process (Li et al., 2024; Sui et al., 2024). Such ther prompting (Baek et al., 2023) or fine-tuning limitations significantly undermine the trustwor- LLMs to learn the underlying structure of KG (Luo thiness of
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：agent-based, encounter difficulties in accurately Figure 1: Challenges for existing KG-enhanced meth- retrieving knowledge and efficiently traversing ods: How to balance faithfulness and efficiency? KGs at scale. In this paper, we propose a uni- fied framework, FiDeLiS1 , designed to improve To address this issue, leveraging knowledge the factuality of LLM responses by anchoring graphs (KGs) as external knowledge sources has answers to verifiable reasoning steps retrieved emerged as a viable solution (Sun et al., 2023; Ma from KGs. To achieve this, we leverage step- et al., 2024; Luo et al., 2024). Unlike traditional wise beam search with a deductive scoring func- retrieval-augmented generation (RAG) that relies tion, allowing the LLM to validate reasoning on web pages or documents (Liu et al., 2024; Qian process step by step, and halt the search once et al., 2024; Bayarri-Planas et al., 2024), KGs repre- the question is deducible. In a
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：KGs, It is defined as, See the prompt in, This step is query, Compared with existing methods, Sun, Table 1, Comparison of FiDeLiS with, LLMs, We replicate the outcomes, ToG and, RoG, We utilize 5 demonstrations, KGQA, Path-RAG, Ablating DVBS, In Table 1, De- mance gains are
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - outperforms strong baselines in both accuracy and ing KGs. It is defined as: given a user query q
  - the query (See the prompt in 搂C.1). This step is query. Compared with existing methods like Sun
  - Table 1: Comparison of FiDeLiS with baseline methods and different backbone LLMs. We replicate the outcomes of ToG and
  - RoG, and retrieve other baseline results directly from the original paper. We utilize 5 demonstrations as our default setting for
  - son results with other baselines over KGQA; (2) remains below using Path-RAG. Ablating DVBS
  - In Table 1, we compare the performance of differ- the results confirm the critical roles of Path-RAG

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1.7x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - fied framework, FiDeLiS1 , designed to improve To address this issue, leveraging knowledge
  - only improve the performance but also enhance Moreover, each fact in a KG can be traced back to
  - limitations significantly undermine the trustwor- LLMs to learn the underlying structure of KG (Luo
  - space, significantly reducing latency without com- Task definition. In this work, we focus on the
  - outperforms strong baselines in both accuracy and ing KGs. It is defined as: given a user query q
  - knowledge graphs to improve factual accuracy. function f can be generally expressed as finding
  - able reasoning, we propose FiDeLiS to improve the Retrieval-Augmented Generation

## 6. Conclusion、局限与可复现性

- **结论段落线索**：in 搂4.4 and Appendix 搂B. search offers several benefits: it (1) enhances the robustness and validity of the final answer by en- Deductive Verification. To ensure that each rea- forcing logical coherence at every step, (2) reduces soning step logically follows from its predeces- computational overhead by pruning unpromising sors and adequately supports the original query, paths early, and (3) mitigates risks such as pre- we leverage the deductive reasoning capabilities of mature termination or excessive extension of the LLMs as a verification criterion (Ling et al., 2023) reasoning process. We provide a concrete example for the beam search process. We first convert the of the deductive verification process in 搂C.6 and user query q into a clear declarative statement q 鈥 , the complete DVBS algorithm in Algorithm 2. 8319 WebQSP CWQ CR-LT WebQSP CWQ CR-LT Ablation Setting Components Methods Backbones Hits@1 (%) Hits@1 (%) Acc (%) Hits@1 (%) Hits@1 (%) Acc (%) No ablation FiDeLiS 79.32 63.1
- **局限/未来工作线索**：limitations significantly undermine the trustwor- LLMs to learn the underlying structure of KG (Luo；ability limitations. As illustrated in Figure 1, how entity.；traceability (Chen et al., 2024). Limitations；hibits certain limitations. Its reliance on external Data Mining, pages 553鈥561.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“FiDeLiS: Faithful Reasoning in Large Language Models for Knowledge Graph Question Answering”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
