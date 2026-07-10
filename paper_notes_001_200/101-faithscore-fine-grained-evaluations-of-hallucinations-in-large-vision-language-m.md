# 101. FaithScore: Fine-grained Evaluations of Hallucinations in Large Vision-Language Models

> 逐篇阅读记录：第 101 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Liqiang Jing, Ruosen Li, Yunmo Chen, Xinya Du
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-emnlp.290)
- **领域标签**：Rationale, Evaluation, MLLM, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 11,416 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：We introduce FAITHSCORE (Faithfulness to Atomic Image Facts Score), a reference-free and fine-grained evaluation metric that measures the faithfulness of the generated free-form answers from large vision-language models
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related Work like METEOR (Banerjee and Lavie, 2005) and；300 LLaVA-1k；50 MiniGPT-4；0 InstructBLIP；2023. Instructblip: Towards general-purpose vision-；2023. Longeval: Guidelines for human evaluation of；1. Large spherical objects painted with vibrant and
- **引言关键线索**：contents denote hallucinations in the answers. FAITH - Large Language Models (LLMs), such as GPT- S CORE allows a more fine-grained and interpretable 3 (Brown, 2020) and ChatGPT (OpenAI, 2022), evaluation of the factual precision of free-form answers. have demonstrated various language modeling ca- pabilities. Despite their achievements, they still et al., 2021). LVLMs have shown strong perfor- lack the capacity to handle multimodal inputs ef- mance on various multimodal tasks, such as Visual fectively. As a result, a significant amount of re- Question Answering (Antol et al., 2015), Image search has shifted its focus towards Large Vision- Captioning (Lin et al., 2014), and Multimodal Con- Language Models (LVLMs) (Liu et al., 2023e; versation (Liu et al., 2023e). Ye et al., 2023; Sun et al., 2023) by incorporat- Despite the effectiveness of LVLMs, the problem ing powerful LLMs (Touvron e
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：potential hazards that could result in significant offer a constricted view on evaluating hallucina- consequences such as misinformation and safety tions, primarily concentrating on coarse-grained concerns, thus degrading the model鈥檚 reliability in object existences (Rohrbach et al., 2018; Lovenia practical applications inevitably (MacLeod et al., et al., 2023), while neglecting other fine-grained 2017). Hence, it is imperative that these issues elements, such as counts, colors, and the interre- are thoroughly measured and addressed (Ji et al., lations between objects (e.g., the spatial relation 2023). between the person and the car in Figure 1), which Nevertheless, there have been limited explo- also form a significant portion of visual hallucina- rations that measure the hallucination problem in tions (Gunjal et al., 2023). Consequently, devising LVLMs. Li et al. (2023c) was among the first a method to holistically evaluate fine-grain
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, MLLM, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Fact Verification. In this, Table 1, Comparison of recognizer LLMs, Table 2, Comparison of the Verifier, LLMs accuracy on and, LVLMs, LLaVA and In-, FAITH S CORE, FAITH - swer length, Lee, Improved baselines with visual, Compared with FAITH S, CORE, CHAIR achieves, Table 7. Traditional metrics
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Fact Verification. In this stage, we compare the
  - Table 1: Comparison of recognizer LLMs鈥 accuracy (%)
  - Table 2: Comparison of the Verifier LLMs accuracy on and human judgment on LVLMs (i.e., LLaVA and In-
  - individual atomic fact derived from the descrip- nificant test between our result and the baseline result is
  - sentence itself is included, there is an improvement FAITH S CORE, we compare it with several multi-
  - comparison of various models in terms of FAITH - swer length on hallucinations, we analyze answer

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - which leaves room for future improvements.
  - fectively. As a result, a significant amount of re- Question Answering (Antol et al., 2015), Image
  - potential hazards that could result in significant offer a constricted view on evaluating hallucina-
  - Nevertheless, there have been limited explo- also form a significant portion of visual hallucina-
  - ing, answering open-domain questions in a free- image, which leaves a large room for improvement.
  - short sentences that are split by punctuation in the the spatial relation. In Figure 1, we show some
  - 鈥測es鈥. Conversely, if the element is in alignment tors. To improve reliability, we only keep these

## 6. Conclusion、局限与可复现性

- **结论段落线索**：content than the other task for most LVLMs. This We introduce a novel metric called FAITH S CORE may be attributed to the fact that captioning sen- for evaluating free-form and open-domain answers tences mainly are brief descriptions and shorter. generated by large vision-language models. Com- The Influence of Multiple Objects. Figure 4 pared to previous metrics, FAITH S CORE offers a shows how the number of objects in the answer finer level of granularity, interpretability, and closer generated by different models affects the FAITH - alignment with human judgments. Our quantita- S CORET虈he model鈥檚 faithfulness varies with the tive analysis demonstrates that current LVLMs are number of objects. While all models start with rel- prone to visual hallucination problems. We also atively high scores when there are few objects in find that the answer length and number of objects the answer, their performance generally drop as the could affect the faithfulness of LVLMs. In addition, number of 
- **局限/未来工作线索**：Limitations Mark S. Krass, Ranjay Krishna, Rohith Kuditipudi,；for future work. Meng Huat Tiong, Junqi Zhao, Weisheng Wang,
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“FaithScore: Fine-grained Evaluations of Hallucinations in Large Vision-Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
