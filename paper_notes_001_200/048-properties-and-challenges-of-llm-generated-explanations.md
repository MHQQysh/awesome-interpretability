# 048. Properties and Challenges of LLM-Generated Explanations

> 逐篇阅读记录：第 48 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jenny Kunz, Marco Kuhlmann
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.hcinlp-1.2)
- **领域标签**：Rationale, Reasoning, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 9,986 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：The self-rationalising capabilities of large language models (LLMs) have been explored in restricted settings, using task-specific data sets.However, current LLMs do not (only) rely on specifically annotated data; noneth
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Work that humans can also provide explanations for in-；4 Experimental Setup；6 Discussion ask the same questions to humans and annotate the；52160. Yuhuai Wu, Sang Michael Xie, Michihiro Yasunaga,；7. If there is an explanation: Is the explanation The linguistic terminology in this example (inde-；3. Increased creativity: Coding allows you to bring；4. More control over technology: With coding skills,
- **引言关键线索**：explanations from the large amount of human text Self-rationalising models produce explanations to- in the training data, including characteristics un- gether with their primary output, often in natural common in the earlier annotated explanations, and language (Marasovic et al., 2022; Wiegreffe et al., in particular properties that contribute primarily to 2022). These models have received increased atten- the communicative function of human explanations tion in recent years as language generation abilities (Lombrozo, 2006; Miller, 2019). Many of these have improved with autoregressive Transformer properties have been argued to be irrelevant or even (Vaswani et al., 2017) architectures, pioneered by detrimental to the goals of explainable NLP, where the GPT models (Radford et al., 2018, 2019). Nat- the aim is to understand how a system arrived at ural language explanations are easily acc
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：cuss the implications of our findings for different math problem with less common numbers: goals of explainable NLP. Question: What is the result of 1/7 + 1/17 + 0.54? 6.1 Properties Answer: 0.8117 In our experiments, we observed that the most pre- Question: Can you explain? valent properties of the explanations are selectivity Answer: The result of 1/7 + 1/17 + 0.54 is 0.8117. and illustrative elements, while subjectivity and This is calculated by adding the fractions 1/7 + misleading explanations occur less often. 1/17 and then adding that sum to 0.54. The sum of The different properties are spread unevenly the fractions is 0.7117, and when this is added to across categories of the dataset. This shows that 0.54 the result is 0.8117. there is not one type of explanation that fits all in- The result is wrong, but when we ask for an ex- struction types and topics. In particular, for math planation, we receive a misleading explanation for
- **方法与解释性关系**：该论文主要围绕 `Rationale, Reasoning, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Figure 2, Comparison of the yes-answers, Brian W Matthews. 1975, Comparison of the predicted
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Figure 2: Comparison of the yes-answers the three an- of explanations for the category (as reported via Q1).
  - Brian W Matthews. 1975. Comparison of the predicted

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - ing data set, we find that generated explanations
  - have improved with autoregressive Transformer properties have been argued to be irrelevant or even
  - used in the construction of Alpaca. However, our with its primary output has been shown to improve
  - ation tasks more often represent questions about improve models, and to discover new knowledge.
  - Asking raters they would give an explanation is not decisions could be improved by this additional in-
  - can improve human-model interaction by increas- may correlate with the likelihood of making new
  - defined as the user鈥檚 confidence that the model 7 Conclusion

## 6. Conclusion、局限与可复现性

- **结论段落线索**：answers for explanations. The natural language explanations given by LLMs The observation that there are no explanations are apparently not faithful to the prediction process for wrong labels is likely a consequence of the spe- but the result of the autoregressive pre-training, i.e. cific data set we use. As Alpaca is LLM-generated, they imitate human explanations from the training it likely only proposes questions and examples that data, possibly constrained by instruction fine-tun- is close to the source model鈥檚 pre-training data, i.e. ing and other alignment techniques. As such, they the instructions are high-probability and are there- exhibit typical properties of human explanations, fore likely to be answered correctly (McCoy et al., which we discuss in 搂6.1. In 搂6.2 we reflect on our 2023). To test this hypothesis, if only anecdotally, evaluation method and data. Finally, in 搂6.3 we dis- we follow McCoy et al. (2023) and construct a cuss the implications of our findings for diffe
- **局限/未来工作线索**：future work. late the result of 0.5 + 0.5 鈭 10 and the explainer；6.2 Limitations of our Method reflected by the user. If the user is competent, their；Limitations of the 59th Annual Meeting of the Association for；In 搂6.2, we discussed the key limitations of our Joint Conference on Natural Language Processing
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Properties and Challenges of LLM-Generated Explanations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
