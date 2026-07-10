# 102. Lookback Lens: Detecting and Mitigating Contextual Hallucinations in Large Language Models Using Only Attention Maps

> 逐篇阅读记录：第 102 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yung-Sung Chuang, Linlu Qiu, Cheng-Yu Hsieh, Ranjay Krishna, Yoon Kim, James Glass
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.84)
- **领域标签**：Probing, Representation, Lens, Hallucination, Hidden, Behavior, Detect
- **本地 PDF 文本规模**：约 11,324 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：When asked to summarize articles or answer questions given a passage, large language models (LLMs) can hallucinate details and respond with unsubstantiated answers that are inaccurate with respect to the input context.Th
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction related to the extent to which an LLM attends to；2 Contextual Hallucinations Detection a feature vector for the time step t:；4 Cross-model Transfer；7 Conclusion；2023. Localizing lying in llama: Understanding in- Debertav3: Improving deberta using electra-style pre-
- **引言关键线索**：the provided contextual information. Concretely, Despite the utility and impressive capabilities of we propose a simple feature called lookback ratio, large language models (LLMs), their tendency to which is computed as the ratio of attention weights generate hallucinations, i.e., content that deviates on the given context versus the newly generated to- from facts or contextually relevant information (Ji kens. At each time step, we calculate this lookback et al., 2023), presents a significant challenge in ratio for each attention head, and train a linear clas- their deployment. In this work, we focus on the sifier, which we call the Lookback Lens, to detect scenarios where the model is provided with the cor- contextual hallucinations based on the lookback rect facts within the input context but still fails to ratio features, as illustrated in Figure 1. The Look- generate accurate outputs
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：such contextual hallucinations. We hypothe- Most prior studies that propose methods to com- size that contextual hallucinations are related bat hallucination focus on the scenario without any to the extent to which an LLM attends to in- input context, where the hallucinations arise from formation in the provided context versus its the LLMs鈥 parametric knowledge. These works own generations. Based on this intuition, we propose a simple hallucination detection model detect and mitigate hallucinations by generally us- whose input features are given by the ratio of ing the LLM鈥檚 representations, such as hidden attention weights on the context versus newly states (Burns et al., 2023; Azaria and Mitchell, generated tokens (for each attention head). We 2023), MLP outputs (Zhang et al., 2024; Simhi find that a linear classifier based on these look- et al., 2024), attention block outputs (Zhang et al., back ratio features is as effective as a ri
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, Lens, Hallucination, Hidden, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：This baseline, Baselines We compare our, Text-based entail- stantial performance, We compare our methods, Baselines To evaluate the, Lookback Lens in generalization, Greedy Decoding, Table 6, Performance comparison on various
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - fier to predict a label of 0 if any hallucinated con- layers instead of the lookback ratio. This baseline
  - Baselines We compare our detection method during the two-fold validation, and present a sub-
  - against several baselines: 1) Text-based entail- stantial performance drop when transferred to out-
  - in a chunk size of 8. We compare our methods with
  - Baselines To evaluate the performance of our pro- to the issue of distribution shift, highlighting the
  - posed method, we compared it against the follow- advantages of Lookback Lens in generalization abil-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X, 30x, 10x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - et al., 2023), presents a significant challenge in
  - 2-7B-Chat (Touvron et al., 2023) to greedy de- results are not significantly better than using the
  - ify the truthfulness of these responses and provide Our results are presented in Table 2. We consider
  - firming a 97% consistency rate between GPT-4o summarization (Sum.). We find that the Lookback
  - the reliability of the automated annotations. We hidden states-based classifier and significantly out-
  - evaluated by GPT-4o, in Table 1. The results show The advantage of the Lookback Lens over the hid-
  - that the generated summaries from LLaMA-2-7B- den states-based classifier is more significant in the

## 6. Conclusion、局限与可复现性

- **结论段落线索**：resulting in a 1024 脳 /D/ matrix and a 1600 脳 /D/ In this section, we further conduct various experi- matrix.2 We then fit a linear regression model to ments and ablation studies on the Lookback Lens map the heads to reconstruct the 7B heads from and its corresponding classifier guided decoding. 13B heads. After applying the linear transformation to the lookback ratio from 13B, the transformed Effect of Chunk Size In Section 3.3 (Table 3), 2 we experiment with chunk size = 8. Here, we study To ensure that two models are generating the same content 3 when extracting lookback ratio, we decode from 7B and run The NQ-train 2.5K data is annotated in the same method the 13B model on the 7B outputs. to annotate NQ testing set, as described in Section 2.2. 1424 the effect of varying chunk sizes, from 4, 8, to 16. Predefined Span We see that there is a slight trend that Lookback Layers QA 鈫 Sum. Sum. 鈫 QA Lens guided decoding prefers shorter chunk size Layer 1-4 69.6 64.0 for NQ and longer chun
- **局限/未来工作线索**：Concretely, we have 1024 heads in 7B and 1600 setting for future work.；interested in how the predictive power is distributed future work. Meanwhile, we visualize the lookback；self-attention module (Vaswani et al., 2017), en- future work.；Limitations to reproduce and distribute reprints for Government
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Lookback Lens: Detecting and Mitigating Contextual Hallucinations in Large Language Models Using Only Attention Maps”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
