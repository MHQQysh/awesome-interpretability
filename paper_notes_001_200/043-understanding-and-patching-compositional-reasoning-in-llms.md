# 043. Understanding and Patching Compositional Reasoning in LLMs

> 逐篇阅读记录：第 43 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Z. Li, Gangwei Jiang, Hong Xie, Linqi Song, Defu Lian, Ying Wei
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.576)
- **领域标签**：Representation, Lens, Reasoning, Hidden, Layer, Control
- **本地 PDF 文本规模**：约 13,562 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：LLMs have marked a revolutonary shift, yet they falter when faced with compositional reasoning tasks.Our research embarks on a quest to uncover the root causes of compositional reasoning failures of LLMs, uncovering that
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；3 Analyzing Compositional Reasoning；5 Locating Important Modules；6 Patching Compositional Reasoning；2021. Show your work: Scratchpads for intermediate Rohan Taori, Ishaan Gulrajani, Tianyi Zhang, Yann；10. As for the matrix used for projecting the im-
- **引言关键线索**：question and explicitly output the step-by-step ra- Compositional reasoning stands as a pivotal mech- tionale with carefully crafted prompting strategies anism, unlocking the ability of learning systems developed by experts (Wei et al., 2022; Zhou et al., to decompose complex tasks into manageable sub- 2023). However, understanding the inherent mech- tasks and tackle them step-by-step (Lu et al., 2023; anism of compositional multi-hop reasoning inside Lake and Baroni, 2023). Despite the revolution- LLMs and enabling them to autonomously rectify ary impact of Large Language Models (LLMs) on their compositional reasoning errors and contin- the NLP landscape, they struggle at basic compo- uously improve over time remain largely under- sitional reasoning tasks (Dziri et al., 2023). This explored. shortcoming is specifically highlighted by Press This work, therefore, sets out to firstly delve
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tional reasoning via editing the located MHSA modules. Our empirical evidence stands tes- tament to CREME鈥檚 effectiveness, paving the two-hop compositional queries, even when they way for autonomously and continuously en- can successfully answer the individual single-hop hancing compositional reasoning capabilities queries that make up the two-hop question. Recent in language models. attempts improve the compositional reasoning ca- pabilities of LLMs by making them decompose the
- **方法与解释性关系**：该论文主要围绕 `Representation, Lens, Reasoning, Hidden, Layer, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Emmanuel Macron, Hasty Answer, LLMs reference, For comparison, Logit, Note of the compositional, Given, We depict the Average, Indirect Effect, AIE, Dataset, Baseline and Evaluation Metric, Accordingly, Meng et al, Io and its corresponding, WOl at the last, Baseline We choose two
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - is Emmanuel Macron (o2 ). Hasty Answer: LLMs reference (h). For comparison, we plot the Logit
  - (in comparison with (f)) to arise the final result o2 . L(Re , hl ) reach a peak and then decline with the
  - and the lower row refers to the comparison group. Note of the compositional query and its corresponding
  - (鈮 0) to 0 for both of the experiment and comparison soning process is usually in smooth going. Given
  - experiment group, we set a comparison group replacement 畏虃lt 鈫 畏lt as p(o2 /畏虃lt 鈫 畏lt ) 鈭 p(o2 ).
  - groups and comparison groups, we observe there We depict the Average Indirect Effect (AIE) of

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - in language models. attempts improve the compositional reasoning ca-
  - the NLP landscape, they struggle at basic compo- uously improve over time remain largely under-
  - soning results through an Intervention (Pearl, 2001; layer) are significantly in charge of properly gener-
  - In Figure 11, we show another example where the layer increasing; (2) the peak of L(Ri , hl ) appears
  - cates the intervention effect is more significant. In each
  - We showcase the effect of CREME in Table 2 and
  - mentations are available in Appendix D. CREME does not aimlessly improve the probabil-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：9672
- **局限/未来工作线索**：et al., 2023; Li et al., 2023b). In the future work, Powered Financial Technologies.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Understanding and Patching Compositional Reasoning in LLMs”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
