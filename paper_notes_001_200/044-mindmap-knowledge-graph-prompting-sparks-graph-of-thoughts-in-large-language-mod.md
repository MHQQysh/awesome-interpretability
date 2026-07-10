# 044. MindMap: Knowledge Graph Prompting Sparks Graph of Thoughts in Large Language Models

> 逐篇阅读记录：第 44 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yilin Wen, Zifeng Wang, Jimeng Sun
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.558)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 11,880 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Large language models (LLMs) have achieved remarkable performance in natural language understanding and generation tasks.However, they often suffer from limitations such as difficulty in incorporating new knowledge, gene
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：5 Conclusion Aakanksha Chowdhery, Sharan Narang, Jacob Devlin,；2022. Progressive prompts: Continual learning for Bosma, Brian Ichter, Fei Xia, Ed Chi, Quoc Le, and
- **引言关键线索**：Scaling large language models (LLMs) to billions 鈥 Hallucination. LLMs are notoriously known to of parameters and a training corpus of trillion produce hallucinations with plausible-sounding words was proved to induce surprising performance but wrong outputs (Ji et al., 2023), which causes in various tasks (Brown et al., 2020; Chowdhery serious concerns for high-stake applications such et al., 2022). Pre-trained LLMs can be adapted as medical diagnosis. to domain tasks with further fine-tuning (Sing- hal et al., 2023) or be aligned with human prefer- 鈥 Transparency. LLMs are also criticized for their ences with instruction-tuning (Ouyang et al., 2022). lack of transparency due to the black-box nature Nonetheless, several hurdles lie in the front of steer- (Danilevsky et al., 2020). The knowledge is im- ing LLMs in production: plicitly stored in LLM鈥檚 parameters, thus infeasi- ble to be v
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Evidence 1 Evidence n prehend KG inputs and infer with a combina- ... Multi-Chain-of-Thought tion of implicit and external knowledge. More- + over, our method elicits the mind map of LLMs, ... ... ... which reveals their reasoning pathways based ... ... on the ontology of knowledge. We evaluate our Subgraph 1 Subgraph n Cirrhosis method on diverse question & answering tasks, especially in medical domains, and show sig- Figure 1: A conceptual comparison between our method nificant improvements over baselines. We also and the other prompting baselines: LLM-only, docu- introduce a new hallucination evaluation bench- ment retrieval + LLM, and KG retrieval + LLM. mark and analyze the effects of different com- ponents of our method. Our results demonstrate 鈥 Inflexibility. The pre-trained LLMs possess out- the effectiveness and robustness of our method dated knowledge and are inflexible to parameter in merging knowledge from LLMs and KGs for 
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Figure 1, We also and the, LLM-only, Table 1, The statistics of the, Synergistic Inference with LLM, KG on model performance, We compare MindMap, GPT-3, BM25 retriever, Ap-, Table 3, The pair-wise comparison by, GPT-4 on the winning, MindMap v.s. baselines on, MindMap vs Baseline GPT-3.5, BM25 Retriever Embedding Retriver, KG Reriever GPT-4 TOT
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - especially in medical domains, and show sig- Figure 1: A conceptual comparison between our method
  - nificant improvements over baselines. We also and the other prompting baselines: LLM-only, docu-
  - Table 1: The statistics of the used datasets. retrieval-based baselines.
  - 3.3.2 Synergistic Inference with LLM and KG on model performance. We compare MindMap鈥檚
  - with various baselines, including vanilla GPT-3.5
  - retrieval-augmented baselines: BM25 retriever,

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：9%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - nificant improvements over baselines. We also and the other prompting baselines: LLM-only, docu-
  - ponents of our method. Our results demonstrate 鈥 Inflexibility. The pre-trained LLMs possess out-
  - combined inference. To reproduce our results
  - gorized into two genres: datasets to illustrate that MindMap outperforms
  - looks to prompt to elicit the intermediate reason- We show the framework of MindMap in Figure 5,
  - E. We show the hallucination influence of results mind map for reasoning, and index the knowledge
  - We find that previous retrieval-augmented LLMs

## 6. Conclusion、局限与可复现性

- **结论段落线索**：the following aspects. mary, an inference process, and a mind map. The summary extracts the accurate result from the mind map, while the inference process displays multiple 4.6.1 How does MindMap perform without reasoning chains from the entities on the evidence correct KG knowledge? graph Gm . The mind map combines all the infer- In Figure 4(c) (Appendix F), when faced with a ence chains into a reasoning graph, providing an in- question where GPT-3.5 is accurate but KG Re- tuitive understanding of knowledge connections in triever errs, MindMap achieves an accuracy rate each step and the sources of evidence sub-graphs. of 55%. We attribute the low accuracy of the KG Retriever to its inability to retrieve the necessary 4.6.5 How does MindMap leverage LLM knowledge for problem-solving. MindMap effec- knowledge for various tasks? tively addresses such instances by leveraging the Figure 4 in Appendix F illustrates MindMap鈥檚 per- LLM inherent knowledge, identifying pertinent ex- formance on
- **局限/未来工作线索**：they often suffer from limitations such as diffi- 鈶 Document retrieval + gpt eye deviation；H Limitations and Potential Risks
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“MindMap: Knowledge Graph Prompting Sparks Graph of Thoughts in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
