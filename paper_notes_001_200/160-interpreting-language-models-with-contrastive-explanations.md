# 160. Interpreting Language Models with Contrastive Explanations

> 逐篇阅读记录：第 160 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Kayo Yin, Graham Neubig
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.emnlp-main.14)
- **领域标签**：Rationale, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 11,168 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Model interpretability methods are often used to explain NLP model decisions on tasks such as text classification, where the output space is relatively small.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction determine certain features of the next token, ruling；5 Do Contrastive Explanations Help；6 What Context Do Models Use for the effect of word embeddings from the effect of；7 Conclusion and Future Work using GPT-2 and GPT-Neo language models, that；2014. Extraction of salient sentences from labelled
- **引言关键线索**：out words that would obviously violate syntax in Despite their success across a wide swath of natural that context (e.g. non 鈥-ing鈥 verbs in the given language processing (NLP) tasks, neural language example). However, this does not explain why the models (LMs) are often used as black boxes: how model made other more subtle decisions, such as they make certain predictions remains obscure (Be- why it predicts 鈥渂arking鈥 instead of 鈥渃rying鈥 or linkov and Glass, 2019). This is in part due to the 鈥渨alking鈥, which are all plausible choices if we only high complexity of the LM task itself, as well as look at the preceding token. In general, language that of the model architectures used to solve it. modeling has a large output space and a high com- We argue that this is also due to the fact that inter- plexity compared to other NLP tasks; at each time pretability methods commonly used in NLP, su
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：to explain NLP model decisions on tasks such Can you stop the dog from as text classification, where the output space 2. Why did the model predict 鈥渂arking鈥 instead of 鈥渃rying鈥? is relatively small. However, when applied to Can you stop the dog from language generation, where the output space 3. Why did the model predict 鈥渂arking鈥 instead of 鈥渨alking鈥? Can you stop the dog from often consists of tens of thousands of tokens, these methods are unable to provide informa- tive explanations. Language models must con- Table 1: Explanations for the GPT-2 prediction given sider various features to predict a token, such the input 鈥淐an you stop the dog from _____". Input to- as its part of speech, number, tense, or seman- kens that are measured to raise or lower the probability tics. Existing explanation methods conflate ev- of 鈥渂arking鈥 are in red and blue respectively, and those idence for all these features into a single expla- with little inf
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：In Figure 2, We compare the effect, Euclidean distance for comparison
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - described above, we also set up a random baseline GN: r=0.73
  - as a comparison, where we create a vector of the
  - do not always outperform the random baseline, con- tween the contrastive and non-contrastive versions of
  - In Figure 2, we see that for most explanation We compare the effect of having no explanation,
  - Euclidean distance for comparison, to disentangle

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - nomena, and that they significantly improve text classification. For example, to explain why an
  - 鈥渢eenagers鈥. We show examples for each linguistic
  - do not always outperform the random baseline, con- tween the contrastive and non-contrastive versions of
  - BLiMP than random vectors for most cases. for each explanation method. Statistically significant
  - score for the analogous explanation method with the tions can improve the ability of users to predict the
  - contrastive explanations particularly outperform 3
  - selected from BLiMP to reflect certain linguistic To test our results for statistical significance and

## 6. Conclusion、局限与可复现性

- **结论段落线索**：have been trained on large amounts of English data. In this work, we interpreted language model de- To reproduce experiments in other languages, we cisions using contrastive explanations by extend- would need a language model in the other language ing three existing input saliency methods to the of sufficient power, which is not available for most contrastive setting. We also proposed three new languages. The experiments in Section 4 address methods to evaluate and explore the quality of con- only a subset of types of grammatical acceptability, trastive explanations: an alignment evaluation to and require a dataset of minimal pairs along differ- verify whether explanations capture linguistically ent types of grammatical acceptability, which may appropriate evidence, a user evaluation to mea- not be available for most languages. Moreover, to sure model simulatability of explanations, and a automatically extract the expected evidence, we clustering-based aggregate analysis to investigate
- **局限/未来工作线索**：7 Conclusion and Future Work using GPT-2 and GPT-Neo language models, that；Future work could explore the application of require human annotators and is therefore more re-；8 Limitations intensive. In total, we computed: 2 explanation；this paper have some limitations, notably their ex- contrastive explanations. For this reason, we omit-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Interpreting Language Models with Contrastive Explanations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
