# 095. LLMs as a synthesis between symbolic and distributed approaches to language

> 逐篇阅读记录：第 95 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Gemma Boleda
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-emnlp.498)
- **领域标签**：Representation, Reasoning, Hidden, Behavior, Analyze
- **本地 PDF 文本规模**：约 10,425 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Since the middle of the 20th century, a fierce battle is being fought between symbolic and distributed approaches to language and cognition.The success of deep learning models, and LLMs in particular, has been alternativ
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction bolic and distributed approaches is, quite fittingly with this；4 Discussion；2022. Idiosyncratic frequency as a measure of deriva-；1988. Regularity and idiomaticity in grammatical ings of the 2018 Conference of the North American；2018 EMNLP Workshop BlackboxNLP: Analyzing of phonology, analogous to those discussed in the
- **引言关键线索**：papper, blurry. Relatedly, within formal linguistics different Since the middle of the 20th century, a fierce bat- approaches have started softening the discreteness of both tle is being fought between two antagonistic ap- categories and rules (see e.g. Erk, 2022, for a comprehensive discussion of probabilistic approaches to semantics and prag- proaches to language and cognition. Although matics). Still, even in these cases the most common approach the specifics vary, they can be broadly character- is to add probabilities or constraints to symbolically-defined rules and categories. ized as follows. Symbolic approaches typically 2 I use 鈥渟ymbolic鈥 and 鈥渄istributed鈥 as umbrella terms, work with discrete, interpretable categories (like with related notions being discrete/categorical/localist for the 鈥渘oun鈥, 鈥渧erb鈥 for parts of speech) and discrete, former, and continuous/fuzzy/sub-symbolic 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Gemma Boleda Universitat Pompeu Fabra / ICREA gemma.boleda@upf.edu Abstract tinuous representations, such as vectors, with con- tinuous functions to combine them, such as those Since the middle of the 20th century, a fierce battle is being fought between symbolic and defined in the different components of a Trans- distributed approaches to language and cog- former.2 nition. The success of deep learning models, The debate between the two approaches has and LLMs in particular, has been alternatively taken different forms in different fields: classicism taken as showing that the distributed camp has vs. connectionism in in cognitive science (Buckner won, or dismissed as an irrelevant engineer- and Garson, 2019), symbolic / rule-based vs. data- ing development. In this position paper, I ar- driven / Machine Learning-based approaches in gue that deep learning models for language ac- tually represent a synthesis between the two AI (Russell an
- **方法与解释性关系**：该论文主要围绕 `Representation, Reasoning, Hidden, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：未自动提取到稳定短句，需回看论文中的 Baselines、Comparison 或 Ablation 小节。

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - clear cases of regularity. In morphosyntax, for all of them, we find a systematic relationship be-
  - in circuits, such as deactivating attention heads. 5 Conclusion
  - 2024). Moreover, we find that derivational mor-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：proaches to language and cognition. Although matics). Still, even in these cases the most common approach the specifics vary, they can be broadly character- is to add probabilities or constraints to symbolically-defined rules and categories. ized as follows. Symbolic approaches typically 2 I use 鈥渟ymbolic鈥 and 鈥渄istributed鈥 as umbrella terms, work with discrete, interpretable categories (like with related notions being discrete/categorical/localist for the 鈥渘oun鈥, 鈥渧erb鈥 for parts of speech) and discrete, former, and continuous/fuzzy/sub-symbolic for the latter. The different terms touch on different properties that for the pur- interpretable rules to combine them (such as those poses of this paper can be lumped together; I will make nu- of formal grammars).1 Distributed approaches in- ances explicit when needed. 3 stead couple uninterpretable high-dimensional con- Functionalists do not use distributed representations, but the issues underlying the divide between formalists and func- 1
- **局限/未来工作线索**：of their roles, it has made much less progress on Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“LLMs as a synthesis between symbolic and distributed approaches to language”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
