# 119. Interpreting Context Look-ups in Transformers: Investigating Attention-MLP Interactions

> 逐篇阅读记录：第 119 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Clement Neo, Shay B. Cohen, Fazl Barez
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.930)
- **领域标签**：Rationale, Hidden, LLM, Layer, Evaluate
- **本地 PDF 文本规模**：约 8,563 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Understanding the inner workings of large language models (LLMs) is crucial for advancing their theoretical foundations and real-world applications.While the attention mechanism and multi-layer perceptrons (MLPs) have be
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2. We develop an automated analysis method；3. We propose an automated approach to evalu-；2 Related Work；3 Background Hence, we can analyze how individual attention；4 Methodology；2 TP + FP TN + FN Table 1: Example of an attention head with semantically；0 Head Explanation Score Head Explanation Score；6 Conclusion
- **引言关键线索**：tion heads activate the neuron only in specific contexts, Transformer-based models have made significant and use GPT-4 to automate this discovery. advancements in natural language understanding and generation. However, the inner workings of large language models (LLMs) remain largely un- 2019) and Pythia (Biderman et al., 2023) by exam- explored, particularly when their behavior deviates ining the associations between their attention heads from common beliefs (Miceli Barone et al., 2023) and multi-layer perceptron (MLP) neurons. Previ- or when modifying their factual knowledge proves ous research has analyzed attention mechanisms challenging (Hoelscher-Obermaier et al., 2023). A and MLPs separately, demonstrating their ability comprehensive understanding of these models is es- to model complex input dependencies and generate sential for expanding their theoretical foundations, text (Ferr
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：find prompts that highly activate them, and de- termine the upstream attention heads respon- + sible. We then generate and evaluate explana- Activate next-token neuron xk When the neuron activates, it tions for the activity of these attention heads in outputs the embedding of an automated manner. Our findings reveal that that token into the residual some attention heads recognize specific con- stream, increasing the MLPk + probability of that token. texts relevant to predicting a token and activate a downstream token-predicting neuron accord- ingly. This mechanism provides a deeper un- unembed The neuron facilitates the model's ability to predict derstanding of how attention heads work with that token over different MLP neurons to perform next-token prediction. logits context or phrases. Our approach offers a foundation for further research into the intricate workings of LLMs and their impact on text generation and under- Figure 1: Our 
- **方法与解释性关系**：该论文主要围绕 `Rationale, Hidden, LLM, Layer, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Baseline Comparisons els of, Addition-, Table 3, Comparison of mean E, Explanation, Activates to introduce examples
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - for 25% to 75% of the max activating prompts (搂5.1), and perform baseline comparisons (搂5.2).
  -   a comparison with a certain speed or level.
  - 5.2 Baseline Comparisons els of different families and sizes, we applied our
  - were higher than the random baseline. Addition-
  - Table 3: Comparison of mean E and skew for different
  - 鈥 Explanation: Activates to introduce examples in comparisons.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - 1 Introduction heads that activate the neuron. We find that some atten-
  - Transformer-based models have made significant and use GPT-4 to automate this discovery.
  - 1. We find that attention heads can recognize 2022a,b). Another approach uses dictionary learn-
  - token, we find its highest scoring neuron. We find that while still making sure it activates the neuron signifi-
  - To find the attention heads that significantly acti-
  - vate the neuron, we find the attention heads whose
  - for a prompt, given only the explanation (The full neurons. For instance, we find heads that activate

## 6. Conclusion、局限与可复现性

- **结论段落线索**：We take the set of attention heads that are active We first identify meaningful attention head activity for 25% to 75% of the max activating prompts (搂5.1), and perform baseline comparisons (搂5.2). for a given neuron. For a given attention head, We then show that this can also be found in smaller we categorize the prompts by head activity. With model variants (搂5.3), We verify the relationship these two classes of prompts, we use GPT-4 to between the attention head and the neuron through generate an explanation for the attention head (the ablation in 搂5.4. We also discuss the interpretabil- full prompt can be found in Appendix C). If the ity illusion in this context (搂5.5) before discussing attention head is indeed active in specific contexts the performance of GPT-4 as an auto-labeler in (e.g. certain phrases), GPT-4 can generate a good these experiments (搂5.6). explanation for this activity. To evaluate the quality of these explanations, we 5.1 Attention Heads May Capture Phrases or 
- **局限/未来工作线索**：(31, 3621, 538) "only" Describes a specific condition, purpose, or limitation；side our scope but merits future work, as discussed unique phrase.；nation for this pattern. Instead, it either offers an Future work could investigate whether this phe-；Limitations Wattenberg. 2021. An interpretability illusion for
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Interpreting Context Look-ups in Transformers: Investigating Attention-MLP Interactions”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
