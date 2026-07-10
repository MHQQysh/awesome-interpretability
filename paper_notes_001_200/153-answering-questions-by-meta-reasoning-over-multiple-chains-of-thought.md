# 153. Answering Questions by Meta-Reasoning over Multiple Chains of Thought

> 逐篇阅读记录：第 153 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ori Yoran, Tomer Wolfson, Ben Bogin, U. Katz, Daniel Deutch, Jonathan Berant
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.364)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 16,772 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Modern systems for multi-hop question answering (QA) typically break questions into a sequence of reasoning steps, termed chain-of-thought (CoT), before arriving at a final answer.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2. Retrieval；2 Background A2: Colonel Walter Phelps served the；3 Method generates an intermediate question qi , based on；4 Experiments and require commonsense or arithmetic reason-；7 Conclusion；8 Limitations Jack Clark, Christopher Berner, Sam McCandlish,；2020 Conference on Empirical Methods in Natural Singh, Hanna Hajishirzi, Luke Zettlemoyer, and；2022. Evaluating explanations: How much do ex- Rohan Taori, Ishaan Gulrajani, Tianyi Zhang, Yann
- **引言关键线索**：In chain-of-thought (CoT) prompting, a large lan- guage model (Brown et al., 2020; Chowdhery et al., which case no significant majority will be formed. 2022; Kadavath et al., 2022; Touvron et al., 2023) Second, focusing exclusively on the final output is prompted to generate its answer following a step- discards relevant information that is present in the by-step explanation (Wei et al., 2022; Nye et al., intermediate reasoning steps. Consider answering 2022). CoT prompting has been shown to dramat- the question 鈥淒id Brad Peyton need to know about ically improve performance on reasoning-heavy seismology?鈥 (Fig. 1). Reasoning chain #1 leads tasks (Kojima et al., 2022; Zhou et al., 2022). Fur- to an incorrect answer (鈥淣o鈥), but its steps provide thermore, Wang et al. (2023) showed that sampling useful information. For example, the intermedi- multiple chains of thought and returning their m
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：鈥 Answer #2: Yes. improve performance, they do not consider Q: What is San Andreas about? A: San Andreas is a film directed by Brad Peyton, about a the relations between intermediate steps across massive earthquake caused by the San Andreas Fault. chains and do not provide a unified explanation Reasoning chain #3 Q: What is Brad Peyton's occupation? for the predicted answer. We introduce Multi- A: Brad Peyton is a film director, writer, and producer. Q: What do film directors have to know? Chain Reasoning (MCR), an approach which Answer #3: No. A: Film directors have to know a lot of things. Q: Is seismology one of the things that Brad Peyton has to prompts large language models to meta-reason know? A: No. over multiple chains of thought, rather than ag- gregate their answers. MCR examines different Multi-Chain Reasoning (MCR) Self-Consistency reasoning chains, mixes information between them and selects the most relevant facts in gen- A
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：MCR outperforms strong baselines, So the final answer, Yes, MCR has three main, To gen- et al, Aly et al, As our baselines, SC by up to, We show that MCR, Union Army throughout the, American, For decomposition prompts, We compare MCR to, We evaluate on, TRATEGY QA, MCR, Explicit Reasoning, Multi-hop questions
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - swer. MCR outperforms strong baselines on 7 So the final answer is: Yes.
  - MCR has three main components (搂3). To gen- et al., 2022; Aly et al., 2021). As our baselines, we
  - chains are then concatenated into a unified multi- baselines, in particular, beating SC by up to 5.7%,
  - 鈥 We show that MCR outperforms all baselines, Union Army throughout the American
  - all baselines. For decomposition prompts, see 搂D.
  - We compare MCR to existing methods on 7 multi- soning chains. We evaluate on: S TRATEGY QA

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：2 points, 3          %, 2          %
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - improve performance, they do not consider Q: What is San Andreas about?
  - swer. MCR outperforms strong baselines on 7 So the final answer is: Yes.
  - guage model (Brown et al., 2020; Chowdhery et al., which case no significant majority will be formed.
  - ically improve performance on reasoning-heavy seismology?鈥 (Fig. 1). Reasoning chain #1 leads
  - jority output further improves accuracy, a method seismology?鈥 constitute an important fact that is
  - (Press et al., 2022; Trivedi et al., 2022a). These sults show MCR consistently outperforms all other
  - 鈥 We show that MCR outperforms all baselines, Union Army throughout the American

## 6. Conclusion、局限与可复现性

- **结论段落线索**：examples): Explanation and Answer errors are 50% on implicit reasoning datasets compared to 23% on This work introduces MCR for meta-reasoning explicit reasoning ones; Retrieval errors are more over multiple reasoning chains. We evaluate MCR prevalent in explicit reasoning tasks with 66% of on 7 datasets for multi-hop QA that require both errors being due to Retrieval or Contradicting facts, implicit and explicit reasoning in an open-domain compared to 30% in implicit datasets. Additional setting and show that it outperforms previous ap- technical details on our analysis are in 搂C.4. proaches on all evaluation benchmarks. 5950
- **局限/未来工作线索**：8 Limitations Jack Clark, Christopher Berner, Sam McCandlish,；text as future work. Due to the inference costs of Wei-Lin Chiang, Zhuohan Li, Zi Lin, Ying Sheng,；we assign a score of 0 when the predicted answer for F EVEROUS. Due to cost limitations, we evalu-；annotation was performed by 4 graduate students in future work could rely on refining generated text
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Answering Questions by Meta-Reasoning over Multiple Chains of Thought”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
