# 040. Investigating Layer Importance in Large Language Models

> 逐篇阅读记录：第 40 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yang Zhang, Yanfei Dong, Kenji Kawaguchi
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.blackboxnlp-1.29)
- **领域标签**：Attribution, Rationale, LLM, Layer, Detect
- **本地 PDF 文本规模**：约 7,201 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Large language models (LLMs) have gained increasing attention due to their prominent ability to understand and process texts.Nevertheless, LLMs largely remain opaque.The lack of understanding of LLMs has obstructed the d
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；4 Estimating Layer Shapley ers and is influenced by its immediate preceding；5 Mechanistic Interpretation via；8 Limitation；2022. Understanding the role of self attention for
- **引言关键线索**：LLMs, which we term cornerstone layers, play The rapid advancement of large language mod- a dominant role in influencing the model鈥檚 perfor- els (LLMs) has revolutionized natural language mance. Notably, removing one of these cornerstone processing, enabling unprecedented capabili- layers can cause a significant performance drop, ties in text generation, translation, and com- reducing the model performance to near random prehension tasksc虄itewei2022chain, hu2021lora, guessing. In contrast, removing other layers typi- rafailov2024direct, ouyang2022training. These cally results in only marginal performance degrada- models, exemplified by architectures such as GPT- tion. We hypothesize that these cornerstone layers 3 (Brown et al., 2020), Llama (Touvron et al., handle some fundamental tasks in LLMs and hope 2023a,b), and Bloom (Workshop et al., 2022), rely this discovery inspires future stu
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：the importance of layers using Shapley values, to addressing the challenges associated with LLMs. a widely used explanation framework in fea- In this paper, we advance the understanding of ture attribution and data valuation. In addi- LLMs by investigating the importance of individual tion, we conduct layer ablation experiments layers in LLMs across multiple tasks. To quantify to assess the performance degradation result- ing from the exclusion of specific layers. Our the contribution of each layer to the overall model findings reveal the existence of cornerstone lay- performance, we extend the Shapley value frame- ers, wherein certain early layers can exhibit a work (Lundberg and Lee, 2017; Ghorbani and Zou, dominant contribution over others. Removing 2020), originally from cooperative game theory, one cornerstone layer leads to a drastic col- to layers in LLMs. We employ an efficient sam- lapse of the model performance, often reducing
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, LLM, Layer, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：未自动提取到稳定短句，需回看论文中的 Baselines、Comparison 或 Ablation 小节。

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：9.4%, 3.5%, 7.5%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - processing, enabling unprecedented capabili- layers can cause a significant performance drop,
  - There is a significant body of research focused on vocabulary V. Each decoder layer Hl consists of an
  - that many heads can be pruned without significant
  - fmodule with skip (x) = fmodule (x) + x. and are significantly larger than LLaMA3-8B. This
  - important layers account for a significant portion of the overall layer importance. For example, in Llama3 70B, the
  - several layers contribute significantly to the model
  - science topics, testing the model鈥檚 ability to com- sess a significantly higher contribution than other

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Attn refers to attention layers, FFN refers to fully connected layers, and MoE refers to Mixture-of-Expert layers. Top 3 Layers Other Layers Cornerstone Layers Llama3 8B 29.1% 70.9% Llama3 8B Attn 0, FFN 0, FFN 1 Llama3 70B 37.0% 63.0% Llama3 70B Attn 0, FFN 0, FFN 3 Mixtral 8x7B 37.5% 62.5% Mixtral 8x7B Attn 0, MoE 0, MoE 1 Table 1: Proportion of Shapley values summarized in Table 2: Identified cornerstone layers. These layers ex- two groups. The top 3 layers with the highest Shapley hibit disproportionately high Shapley values compared values account for 30% of the total Shapley value. In to other layers across various tasks. larger models such as Llama3 70B and Mixtral 8x7B, the proportion of Shapley values attributed to the top three layers is even higher compared to smaller models. Are there critical layers? According to Figure 2 and Table 1, we observe a clear phenomenon that several layers contribute significantly to the model sesses the model鈥檚 understanding of physical com- pe
- **局限/未来工作线索**：One limitation of the Shapley value is that, on its；7 Conclusion and Future Work our Hypothesis 2. Furthermore, a deeper explo-；performance. Future work on layer functionalities；Future works can continue the study on layer
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Investigating Layer Importance in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
