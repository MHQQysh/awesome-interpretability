# 188. Journey to the Center of the Knowledge Neurons: Discoveries of Language-Independent Knowledge Neurons and Degenerate Knowledge Neurons

> 逐篇阅读记录：第 188 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yuheng Chen, Pengfei Cao, Yubo Chen, Kang K. L. Liu, Jun Zhao
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/29735/31264)
- **领域标签**：Attribution, Hallucination, Hidden, Behavior, Detect
- **本地 PDF 文本规模**：约 7,119 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Pre-trained language models (PLMs) contain vast amounts of factual knowledge, but how the knowledge is stored in the parameters remains unclear.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 English Potential DKN 3
- **引言关键线索**：in the model parameters, where such parameters are named Pre-trained language models (PLMs) (Devlin et al. 2018; Knowledge Neurons (Dai et al. 2022). Radford et al. 2019; Shliazhko et al. 2022; OpenAI 2023; Touvron et al. 2023; Wang et al. 2023) have revolutionized Recently, several established approaches strive to elucidate the field of natural language processing, due to their excep- the knowledge storage mechanism in PLMs. One strategy tional performance across a broad spectrum of tasks. These is the gradient-based method (Ancona et al. 2019), which models, trained on extensive corpora such as Wikipedia, are assesses the contribution of each neuron by calculating its widely believed to encapsulate vast amounts of factual knowl- attribution score using integrated gradients. Another is the edge (Petroni et al. 2019b; Jiang et al. 2020), but how the causal-inspired method (Cao et al. 202
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：izes knowledge neurons more precisely compared to current cess and functionality. methods, and is more universal across various architectures and languages. Moreover, we conduct an in-depth exploration of knowledge neurons, leading to the following two impor- Locate KN DKN KN Filter tant discoveries: (1) The discovery of Language-Independent PLMs Knowledge Neurons, which store factual knowledge in a form The capital of Tanzania is ___ Dodoma that transcends language. We design cross-lingual knowledge editing experiments, demonstrating that the PLMs can accom- plish this task based on language-independent neurons; (2) The discovery of Degenerate Knowledge Neurons, a novel (b) Degenerate Knowledge Neurons: Acquisition process and type of neuron showing that different knowledge neurons can functionality. 鈥淥N鈥 indicates the PLMs must activate at least one store the same fact. Its property of functional overlap endows corresponding degenerat
- **方法与解释性关系**：该论文主要围绕 `Attribution, Hallucination, Hidden, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Attr, Pn i, The ideal baseline vector, Xu et al. 2023, Wang, Lipton, Enguehard, PLMs, We define neurons that, To identify these type, PLM architectures. Let the, B-KN is the baseline, B-KN, The findings from
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - qi , and its associated baseline vector w鈥 j corresponds to qi鈥 .
  - where wj is the value of wj , w鈥 j is the baseline vector Attr(wjl ) = Pn i=1
  - integrating the gradients. The ideal baseline vector w鈥 j ,
  - an architecture adaptation technique to compute baseline els is language-independent (Xu et al. 2023; Wang, Lipton,
  - baseline vectors, we follow the method of Enguehard(2023), knowledge in multilingual PLMs. We define neurons that
  - we meticulously design the baseline vectors for different To identify these type of knowledge neurons, we devise a

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - ability to draw multilingual conclusions. knowledge neurons. In detail, we first localize the knowledge
  - in sub-optimal performance. We address this by introducing significant portion of factual knowledge within these mod-
  - ferent languages varies, resulting in significant differences in rons (D)
  - an intriguing phenomenon: distinct sets of neurons are respon- significant decrease in prediction probability; whereas Equa-
  - for a specific fact denoted as 鉄╤, r, t鉄, suppose we localize lead to a significant decrease in prediction probability. This
  - both subsets of N , we observe no significant decrease in pre- we need to evaluate all possible subsets, the complexity of
  - redundant. That is to say, besides the fact 鉄╤, r, t鉄, A may and record neurons that do not cause a significant decrease in

## 6. Conclusion、局限与可复现性

- **结论段落线索**：In order to localize knowledge neurons more precisely, neurons, then aggregate and filter them to obtain degenerate we follow the gradient-based method and propose a novel knowledge neurons. For the query 鈥淭he capital of Tanzania is knowledge localization method, termed Architecture-adapted 鈥, the PLM must activate at least one corresponding degen- Multilingual Integrated Gradients (AMIG). Firstly, for the erate knowledge neuron to predict the correct fact Dodoma. lack of universal method in different PLM architectures, we Intuitively, the property of functional overlap in degenerate design an architecture adaptation technique, making the base- knowledge neurons endows the PLMs with a robust under- line vectors in the integrated gradients algorithm (Lundstrom, standing of factual knowledge, ensuring that its mastery of Huang, and Razaviyayn 2022) universally compatible across facts remains stable and less prone to errors. Inspired by this, different PLM architectures. Secondly, for the
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Journey to the Center of the Knowledge Neurons: Discoveries of Language-Independent Knowledge Neurons and Degenerate Knowledge Neurons”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
