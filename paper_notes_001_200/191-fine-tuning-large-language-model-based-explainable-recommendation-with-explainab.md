# 191. Fine-Tuning Large Language Model Based Explainable Recommendation with Explainable Quality Reward

> 逐篇阅读记录：第 191 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Mengyuan Yang, Mengying Zhu, Yan Wang, Linxun Chen, Yi‐Lei Zhao, Xiuyuan Wang, Bing Han, Xiaolin Zheng, et al.
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/28777/29489)
- **领域标签**：Rationale, Evaluation, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 8,318 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Large language model-based explainable recommendation (LLM-based ER) systems can provide remarkable human-like explanations and have widely received attention from researchers.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Item feature: screen, character, action, wolverine, scenes, intense, future；2 Item feature: screen, character, action, wolverine, scenes, intense, future；3 Item feature: George Lucas, theme, previous, war
- **引言关键线索**：Explainable recommendation (ER) systems aim to provide problem, making this explanation unable to improve user high-quality explanations to help users understand the rec- satisfaction. Moreover, the widely recognized issue of hal- ommendations and make decisions. According to the phe- lucination (Ji et al. 2023) in LLM can lead to factual inac- nomenon reported in an exhaustive survey of explanation curacies within explanations, thereby compounding the low- quality (Lu et al. 2023), generating human-like explanations quality problem. Therefore, directly using an LLM to fulfill can significantly improve the adoption rate of recommended the ER task cannot be one-size-fits-all. items. Among various explanation forms, such as tags (Yan With the above insightful study of LLM-based ER, we et al. 2020), reasoning paths (Wang et al. 2019) and images analyze that the causes of the low-quality pro
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：reduction in the precision of the generated explanations. fluent, diverse, informative, and highly personalized expla- Cause 3: Lack of sufficient high-quality explanation data nations. for fine-tuning limits the adaptability of LLM to the ER task. To better adapt to the ER task, LLM requires fine-tuning with explanation data. Existing studies (Hada and Shevade Related Work 2021; Li, Zhang, and Chen 2023) fine-tune LLM by aligning paired generated explanation and review from the same user- With natural language processing technology having item pair, i.e., denoting the review as the ground truth expla- achieved remarkable performance in text generation, ER nation. However, the review dataset is questionable, and the based on text generation has received increasing attention ground truth corresponding to a generated explanation is not because of its good human-like language generation capabil- necessarily of high quality. For example, we
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We adopt a token-level, Comparison Methods. To demonstrate, HQAR, LLM2ER-EQR, LM-ER baselines, Evaluation Metrics. We evaluate, We compare LLM2ER-EQR with, LM-ER meth-, BLEU, Chen and LLM2ER-EQR consistently, In terms of the, Secondly, LLM2ER-EQR exhibits the, Uvtest, CCR model in, Improvement of LLM2ER-EQR over, Table 2, Performance comparison of all
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - explanation came from H. We adopt a token-level binary Comparison Methods. To demonstrate the effectiveness
  - cross-entropy loss as a discriminator loss to train the HQAR, of LLM2ER-EQR, we compare seven LM-ER baselines,
  - Evaluation Metrics. We evaluate the performance of ex- We compare LLM2ER-EQR with seven LM-ER meth-
  - widely used evaluation metrics, i.e., BLEU (Chen and LLM2ER-EQR consistently outperforms baselines on all
  - In terms of the explanation quality perspective, we adopt performing baseline. Secondly, LLM2ER-EQR exhibits the
  - sentence-wise diversity and propose a new metric named scores among all baselines, with an average improvement of

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1          X, 1X, 1           X, 1            X, 5 times, 1                     X, 1       X, 1                   X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Explainable recommendation (ER) systems aim to provide problem, making this explanation unable to improve user
  - can significantly improve the adoption rate of recommended the ER task cannot be one-size-fits-all.
  - an LLM. In Figure 1, we show an ER process for a movie personalized user鈥搃tem information is not integrated into
  - dual sides will mix in some words that do not match user world datasets, which demonstrate our model significantly
  - preferences and item features simultaneously, resulting in a outperforms the state-of-the-art methods and can generate
  - In order to improve the quality of explanations for LLM- ized, convincing explanations. Subsequently, there are meth-
  - trastive learning (for addressing Cause 2); (2) high-quality improve recommendation performance rather than generat-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Legacies of Language Models. It is worth noting that a tributes to guide LLM generate explanations in the direction large part of our framework relies on the pre-trained LLMs without harmful information. Nonetheless, we must admit (Brown et al. 2020). This means that LLM2ER naturally in- that when actually deploying our model in a real scenario, herits the advantages and disadvantages of the pre-trained it is necessary to take post-processing to further prevent the LLM. Specifically, the advantage is that LLM2ER-EQR can display of harmful information to users. utilize the implicit knowledge existing in the pre-trained LLM to generate explanations, while the disadvantage is Conclusions and Future Work. In this paper, we pro- that it may introduce risks, such as potentially producing of- pose a novel LLM-based ER backbone LLM2ER and fine- fensive language, propagating social biases and stereotypes, tune such a backbone as LLM2ER-EQR in an RL paradigm and leaking private information (Weid
- **局限/未来工作线索**：LLM to generate explanations, while the disadvantage is Conclusions and Future Work. In this paper, we pro-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Fine-Tuning Large Language Model Based Explainable Recommendation with Explainable Quality Reward”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
