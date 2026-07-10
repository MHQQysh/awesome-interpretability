# 113. Backward Lens: Projecting Language Model Gradients into the Vocabulary Space

> 逐篇阅读记录：第 113 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Shahar Katz, Yonatan Belinkov, Mor Geva, Lior Wolf
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.142)
- **领域标签**：Attribution, Representation, Lens, Hidden, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 15,747 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Understanding how Transformer-based Language Models (LMs) learn and recall information is a key goal of the deep learning community.Recent interpretability methods project weights and hidden states obtained from the forw
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related Work characteristic for interpretability study or predict-；4 Backward Lens In our analysis we focus on the MLP layers, due；5 Understanding the Backward Pass；8 Conclusions different edits and tasks remains an open question,
- **引言关键线索**：information that F F1 encountered during the forward Deep learning models consist of layers, which are pass. Utilizing a vocabulary projection method, we parameterized by matrices that are trained using a reveal that this information represents the token 鈥渢eam鈥. method known as backpropagation. This process The gradients of the second MLP matrix, F F2 , aim to involves the creation of gradient matrices that are shift the information encoded within F F2 towards the embedding of the new target. used to update the models鈥 layers. Backpropaga- tion has been playing a major role in interpreting deep learning models and multiple lines of study ag- gregate the gradients to provide explainability (Si- thousands of neurons in each layer, while certain monyan et al., 2014; Sanyal and Ren, 2021; Chefer features are likely distributed across multiple neu- et al., 2022; Sarti et al., 2023; Miglani et
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：weights and hidden states obtained from the forward pass to the models鈥 vocabularies, help- ing to uncover how information flows within LMs. In this work, we extend this method- ology to LMs鈥 backward pass and gradients. We first prove that a gradient matrix can be cast as a low-rank linear combination of its for- ward and backward passes鈥 inputs. We then develop methods to project these gradients into vocabulary items and explore the mechanics Figure 1: An illustration depicting the tokens promoted of how new information is stored in the LMs鈥 by a single LM鈥檚 MLP layer and its gradient during the neurons. Our code is available at: https: forward and backward pass when editing the model to //github.com/shacharKZ/BackwardLens . answer 鈥淧aris鈥 for the prompt 鈥淟ionel Messi plays for鈥. The gradients (in green) of the first MLP matrix, F F1 ,
- **方法与解释性关系**：该论文主要围绕 `Attribution, Representation, Lens, Hidden, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：VJP, Figure 7, VJPs, F2 . In, Figure 8, F2 . It is, Figure 9, The inputs xi, Similar comparison is conducted, Comparison with Naive Backpropagation, Our baselines for comparison, The comparison was conducted
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - of the last token鈥檚 input and VJP, x鈯n 路 未n , and that a given prompt, but it falls short in comparison
  - Figure 7: VJPs (未i , the spanning set of F F2 ) and single gradient鈥檚 neurons comparison for layer 14鈥檚 F F2 . In
  - Figure 8: VJPs (未i ) and single gradient鈥檚 neurons (sorted by norm) comparison for layer 14鈥檚 F F2 . It is noteworthy
  - Figure 9: The inputs xi (which are F F1 鈥檚 spanning set) and single gradient鈥檚 neurons (sorted by norm) comparison
  - more than others. Similar comparison is conducted
  - of plotting the norm of the VJPs, we compare the

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：4096 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - pass. (iii) Investigating the embedding of these sets bedded target. We show that gradients work in
  - with a single forward pass and an approximation multi-step (iterative) execution. Overall, our results
  - 8 Conclusions different edits and tasks remains an open question,
  - an interpretable way. As we show, the gradients are constrained embedding space.
  - and Mor Geva. 2023. Jump to conclusions: Short-
  - In our study, especially in our figures and tables, forward pass. Furthermore, our results in Section 6,
  - In conclusion, In this section, we demonstrate

## 6. Conclusion、局限与可复现性

- **结论段落线索**：which has been recently discussed in the context Other LL-type interpretability contributions shed of superposition (Elhage et al., 2022). While most light on LMs through the forward pass. Here, we work has examined superposition phenomenon by show that gradients can be projected into the vocab- looking at the forward pass of LMs, exploring how ulary space and utilize the low-rank nature of the information is embedded into the same subspaces gradient matrices to explore the backward pass in might shed light on how LMs work within their an interpretable way. As we show, the gradients are constrained embedding space. best captured by a spanning set that contains either In addition, in this work we utilize Logit-Lens the input to each layer, or its VJP. These two com- (LL) projection, which is known to have reduced in- ponents, which are accessible during the forward terpretability abilities in the earlier layers of models and backward passes, are used to store information compared to lat
- **局限/未来工作线索**：editing methods. 9 Limitations；Future Work: The focus of this work is in under- Our use of LL in projecting gradients has limita-；one of our method鈥檚 limitations lies in its per-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Backward Lens: Projecting Language Model Gradients into the Vocabulary Space”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
