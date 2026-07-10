# 103. Neuron-Level Knowledge Attribution in Large Language Models

> 逐篇阅读记录：第 103 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Zeping Yu, Sophia Ananiadou
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.191)
- **领域标签**：Attribution, Hidden, LLM, Layer, Control
- **本地 PDF 文本规模**：约 9,365 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Identifying important neurons for final predictions is essential for understanding the mechanisms of large language models.Due to computational constraints, current attribution techniques struggle to operate at neuron le
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Work In this section, we aim to locate important neurons
- **引言关键线索**：Transformer-based large language models (LLMs) (c) (Brown et al., 2020; Ouyang et al., 2022; Chowd- (b) hery et al., 2023) possess remarkable capabilities for storing factual knowledge, which is important for downstream tasks including question answer- (a) ing (Jiang et al., 2021) and reasoning (Rajani et al., 2019). While recent studies (Dai et al., 2021; Meng et al., 2022; Geva et al., 2023; Yu et al., 2023; Chen The capital of France is et al., 2024) have made significant progress in un- derstanding knowledge localization and the infor- Figure 1: (a) Query neurons in shallow FFN layers. (b) mation flow from inputs to predictions, it is still Attention query/value neurons in attention heads. (c) hard to identify exact parameters for knowledge Value neurons in deep FFN layers. storage in LLMs due to several reasons. Firstly, ex- In this paper, we focus on neuron-level attribu- isting st
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Identifying important neurons for final predic- plicability to millions of neurons in LLMs, which tions is essential for understanding the mecha- nisms of large language models. Due to com- are proved as fundamental units for storing knowl- putational constraints, current attribution tech- edge (Geva et al., 2020; Dai et al., 2021; Geva et al., niques struggle to operate at neuron level. In 2022; Nanda et al., 2023b). Secondly, while a few this paper, we propose a static method for studies (Dar et al., 2022; Geva et al., 2022) have pinpointing significant neurons. Compared devised methods for analyzing neurons, they often to seven other methods, our approach demon- lack comparisons with other methods. Therefore, strates superior performance across three met- how to identify important neurons in LLMs is still rics. Additionally, since most static methods unclear. Lastly, existing methods typically concen- typically only identify "value n
- **方法与解释性关系**：该论文主要围绕 `Attribution, Hidden, LLM, Layer, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Therefore, Compared with seven other, Compared with seven static, El-, Compared with, In this section, Comparison of Attribution Methods, We compare the proposed, Table 1. In comparison, Attribution methods. We compare, GPT2, Llama-7B, Compared with proba- exploration
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - to seven other methods, our approach demon- lack comparisons with other methods. Therefore,
  - dictions. Compared with seven other static meth- is calculating how much an internal module affects
  - Compared with seven static methods, our method tion heads鈥 roles in different cases and tasks (El-
  - stantial roles. Compared with [3, 1, 1, 1], [6, 2, 2, 2]
  - In this section, we compare our neuron-level attri- these neurons by setting the top10 neurons鈥 param-
  - 4.1 Comparison of Attribution Methods

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - pinpointing significant neurons. Compared devised methods for analyzing neurons, they often
  - et al., 2024) have made significant progress in un-
  - Zhao et al., 2024; Wu et al., 2024a) point out that into vocabulary space, play significant roles. Based
  - neurons that contribute significantly to final pre- widely utilized for this purpose. The core idea
  - or query neurons (1000) can significantly influ- Mechanistic interpretility (Olah, 2022; Nanda et al.,
  - components of v are significant for amplifying the
  - the highest. The probability of each token for x Another significant finding is that both the coef-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：26 , a32 , a25 and a22 rank top8 for lan- 6th head), a17 11 13 17 icance for future studies delving into neuron-level guage, capital and country. Similarly, a1223 , a19 , a31 , 31 25 knowledge editing. 3273 top10 query layers for FFN neurons lang capi cnty col num mon lang a26 , a22 , a19 , f26 , a17 , f25 , f27 , f23 , a25 , a23 G 91/96 98/99 89/94 95/97 96/96 83/88 capi a26 , a22 , a19 , a17 , f25 , a23 , a18 , f26 , f21 , a28 L 78/92 84/93 91/98 85/93 90/96 94/98 cnty a26 , a22 , a17 , a19 , a23 , f18 , a20 , a18 , f21 , f25 col f26 , f29 , a26 , f28 , f25 , a22 , f27 , a17 , a24 , f23 Table 7: MRR/probability decrease (%) when interven- num f26 , f23 , f27 , a22 , f25 , a17 , f19 , f21 , a23 , f0 ing 1,000 query neurons in GPT2 (G) and Llama (L). mon a17 , a22 , f23 , a26 , f26 , f24 , a19 , a20 , f20 , f21 for each sentence, both MRR and probability drops lang f21 , a16 , a19 , f18 , a18 , a21 , a17 , f30 , f19 , a14 very much (92%/95% in GPT2 and 87%/95% in capi a18 , a16 , f23 ,
- **局限/未来工作线索**：be a valuable direction in future works.；6 Limitations Olah. 2023. Towards monosemanticity: Decom-；The first limitation of our study is that it focuses on Transformer Circuits Thread. Https://transformer-；explore these areas in future work. tificial Intelligence, volume 38, pages 17817鈥17825.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Neuron-Level Knowledge Attribution in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
