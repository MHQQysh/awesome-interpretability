# 019. Sui Generis: Large Language Models for Authorship Attribution and Verification in Latin

> 逐篇阅读记录：第 19 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Svetlana Gorovaia, Gleb Schmidt, Ivan P. Yamshchikov
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.nlp4dh-1.39)
- **领域标签**：Attribution, Steering, LLM, Behavior, Control
- **本地 PDF 文本规模**：约 8,370 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：This paper evaluates the performance of Large Language Models (LLMs) in authorship attribution and authorship verification tasks for Latin texts of the Patristic Era.The study showcases that LLMs can be robust in zero-sh
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction datasets, along with their advanced inference ca-；2. LIMITED: the models get a general descrip- Leander of Seville 13,077；3. HIP: historically informed prompting, when；6 Discussion formal instructions are provided for tasks that are；2017. Convolutional neural networks for authorship
- **引言关键线索**：pabilities and their ability to provide human-like Unlike in computational linguistics, authorship natural language explanations for their outputs in- analysis in the field of digital humanities still evitably raise the question of how these systems largely relies on the complicated process of domain- can be leveraged for philological and historical in- specific manual feature engineering (Corbara et al., vestigations. 2020; Manousakis and Stamatatos, 2023; Corbara To promote the wider adoption of large language et al., 2023; Cl茅rice and Glaise, 2023; Beullens models (LLMs) as research tools in the digital hu- et al., 2024). This is mostly due to the fact that manities, this study assesses the zero-shot perfor- the predictions made by machine learning models mance of several publicly available, state-of-the-art with regard to philological and historical authorship LLMs 鈥 namely GPT-4o, G
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：able to beat, under certain circumstances, the (Setzu et al., 2024) nor exhaustive, they represent a traditional baselines, obtaining a nuanced and truly explainable decision requires at best a lot significant advancement in the field of explainabil- of experimentation. ity. The linguistic 鈥渒nowledge鈥 of LLMs, acquired through training on extensive multilingual textual
- **方法与解释性关系**：该论文主要围绕 `Attribution, Steering, LLM, Behavior, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We compare their performance, POS n-grams, Stamatatos, These features were often, LLMs to be the, Metrics and Baselines, For, GPT-4o outperformed both baselines, In all
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - traditional baselines, obtaining a nuanced and
  - and attribution. We compare their performance frequencies, and POS n-grams (Stamatatos, 2013).
  - against traditional baselines, including classical These features were often combined with feature
  - of LLMs to be the challenge posed by sample it to conventional baselines but also to study the
  - the models get a general description of the task 3.1 Metrics and Baselines
  - different baselines, (four baselines in total). For

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significant advancement in the field of explainabil-
  - ers have proven outperform the previous methods,
  - mont et al., 2018, 2020). Yet, this improvement was
  - significantly improve the precision of the model鈥檚
  - We find a compelling reason to explore the use only to measure LLMs鈥檚 performance and compare
  - some chunks are slightly longer or shorter than the Both models outperformed the LaBerta-based base-
  - ples was done for each task independently. not outperform the baselines in terms of recall and

## 6. Conclusion、局限与可复现性

- **结论段落线索**：resistant to formalization (Ouyang et al., 2022; Liu The experiment have provided interesting insights et al., 2021). into the capabilities of the LLMs and the way how A closer examination of the models鈥 output7 they approach the tasks of authorship verification highlights this issue. When explicitly instructed, and attribution. the models generally perform well in identifying In Authorship Verification, the strong perfor- the specified features. For instance, they demon- mance of GPT-4o in the basic setting was largely strated notable 鈥渁ttention鈥 to syntactical patterns anticipated due to its advanced capabilities. How- such as anaphora (repetition of a word or phrase ever, the comparable results achieved by Claude- at the beginning of successive clauses), asyndeton 3.5 are noteworthy, indicating its potential effec- (omission of conjunctions), polysyndeton (repeti- tiveness in authorship verification tasks. tion of conjunctions), and hyperbaton (disruption We were initially concerned
- **局限/未来工作线索**：several limitations one has to keep in mind. First, Diogo Almeida, Janko Altenschmidt, Sam Altman,；guistics (ACL). We acknowledge the limitations in- Shari Boodts and Gleb Schmidt. 2022. Ser-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Sui Generis: Large Language Models for Authorship Attribution and Verification in Latin”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
