# 114. Towards Interpretable Sequence Continuation: Analyzing Shared Circuits in Large Language Models

> 逐篇阅读记录：第 114 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Michael Lan, Philip H. S. Torr, Fazl Barez
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.699)
- **领域标签**：Mechanistic, Representation, Evaluation, Hidden, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 14,630 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Mechanistic为主线，结合论文摘要中的核心设定：While transformer models exhibit strong capabilities on linguistic tasks, their complex architectures make them difficult to interpret.Recent work has aimed to reverse engineer transformer models into human-readable repr
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction assists in aligning AI via methods such as model；2) The finding of similar sub-circuits across vations of certain components with other values,；1. Connectivity Discovery consists of applying；7 Ablating Sequence Continuation；8 Conclusion；2023. Activation addition: Steering language models can be filled in with sequence members (Febru-；I Ablation on Math-Related Prompts
- **引言关键线索**：editing (Meng et al., 2023), which precisely targets Transformer-based large language models (LLMs) problematic areas for more efficient re-alignment like GPT-4 have demonstrated impressive natu- without erroneously altering healthy components. ral language capabilities across a variety of tasks Documenting the existence of shared circuits en- (Brown et al., 2020; Bubeck et al., 2023). How- ables safer, more predictable model editing with ever, these models largely remain black boxes due fewer risks, as editing a circuit may affect another to their complex, densely connected architectures. if they share sub-circuits (Hoelscher-Obermaier Understanding how these models work is important et al., 2023). Therefore, interpretability enables for ensuring safe and aligned deployment, espe- safer alignment by understanding adverse effect cially as they are already being used in high-impact preven
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：rely on shared circuit subgraphs with analo- subgraphs of neural networks that represent algo- gous roles. Additionally, we show that this sub-circuit has effects on various math-related rithmic tasks (Elhage et al., 2021). Recent work prompts, such as on intervaled circuits, Span- in interpretability has uncovered transformer cir- ish number word and months continuation, and cuits that implement simple linguistic tasks, such natural language word problems. This mecha- as identifying indirect objects in sentences (Wang nistic understanding of transformers is a critical et al., 2022). However, only a few studies have fo- step towards building more robust, aligned, and cused on the existence of shared circuits (Merullo interpretable language models. et al., 2023), in which circuits utilize the same sub- circuits for similar tasks. Identifying shared circuits
- **方法与解释性关系**：该论文主要围绕 `Mechanistic, Representation, Evaluation, Hidden, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：MLP, We continue it- Task, Comparison, We compare increasing se-, We compare our com-, Comparison Statistics. We have, Concrete problems in ai, Learning, Todor Mihaylov, Pushkar Mishra, Igor Moly- the, In Table 4, Last Token Sequence Detection, Head, In comparison
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tention heads, and then its MLP. We continue it- Task Comparison. We compare increasing se-
  - plete correctly after ablation. We compare our com-
  - Comparison Statistics. We have shown that Concrete problems in ai safety. arXiv: Learning,
  - tinet, Todor Mihaylov, Pushkar Mishra, Igor Moly- the "big picture" comparison of scores and on the
  - In Table 4, we compare every task鈥檚 circuit with G.1 Last Token Sequence Detection Head
  - model. In comparison, on average, removing any head from the fully unablated circuit shown in each

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - gous roles. Additionally, we show that this
  - seeks to automate finding the connectivity of circuit tion experiment to improve the Colored Objects
  - without head decomposition). We find our chosen granularity samples per task, with a total of 4608 samples.
  - Continuation Tasks recent elements are more significant. This informa-
  - 4.4, 7.11 and 9.1, which we show to be important
  - MLP Connectivity. For all tasks, we find that for attention head 4.4, Months attend to Months,
  - Due to the (qkv) circuit鈥檚 large display size, we show the

## 6. Conclusion、局限与可复现性

- **结论段落线索**：about these attention patterns are in Appendix M. In this section we analyze main results, and include Last Token Sequence Detection Head. In extended results in Appendix 搂G. Figure 3 and in Appendix 搂G, we describe results 5.1 Sub-Circuit Hypothesis that show how attention head 7.11 acts to transfer the detected sequence information to the last to- We hypothesize the important shared components ken, which aids attention head 9.1 and MLP 9 in for the three tasks work together as a functional determining the successor member. 4 Due to the (qkv) circuit鈥檚 large display size, we show the circuits with (qkv) decomposition in Appendix K. 5.3 Successor Components 5 20% is chosen due to using Tn = 80%, so that for many removal order variations, a component with a 20% importance Figure 1 shows that attention head 9.1 receives in- cannot be removed, unless there are alternative backups. formation from both attention heads 4.4 and 7.11. 12580 Table 1: Drop in Task Performance for GPT-2 Small whe
- **局限/未来工作线索**：ponent set ablations with random component abla- 9 Limitations；On intervaled sequences. As shown in Table 3, We discuss limitations of this paper in this section.；experiments on intervaled sequences. Future work；Future Work. Future work can build upon
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Towards Interpretable Sequence Continuation: Analyzing Shared Circuits in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
