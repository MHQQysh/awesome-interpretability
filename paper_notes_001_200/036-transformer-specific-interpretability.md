# 036. Transformer-specific Interpretability

> 逐篇阅读记录：第 36 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Hosein Mohebbi, Jaap Jumelet, Michael Hanna, Afra Alishahi, Willem Zuidema
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.eacl-tutorials.4)
- **领域标签**：Analysis, Transformer, Behavior, Analyze
- **本地 PDF 文本规模**：约 3,416 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Hosein Mohebbi, Jaap Jumelet, Michael Hanna, Afra Alishahi, Willem Zuidema.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Tutorial Description；3 Target Audience；4. Coffee break；4 Outline of Tutorial Structure；6 Special Requirements on understanding the abilities of pre-trained lan-；8 Tutorial Instructors；2021. Post-hoc interpretability for neural nlp: A and Trustworthy Machine Learning (SaTML), pages
- **引言关键线索**：Transformers, which serve as the backbone archi- 鈥 Model-agnostic approaches: probing, tecture of modern NLP systems. feature attributions, behavioral studies The only ACL tutorials similar to ours are "Inter- 鈥 How are model-agnostic approaches pretability and Analysis in Neural NLP" (Belinkov adapted to Transformers? What are their et al., 2020) and "Fine-grained Interpretation and limitations? Causation Analysis in Deep NLP Models" (Saj- jad et al., 2021), held at ACL 2020 and NAACL 2. 30 minute lecture on interpretation of atten- 2021, respectively. Both focused on general model- tion and context mixing: agnostic interpretability techniques. Our tutorial, 鈥 Attention analysis (Clark et al., 2019) as however, will question the effectiveness of those a straightforward starting point for mea- general-purpose analysis methods and mark the suring context mixing. next chapter: a transition
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：present Transformer-specific interpretability cisely tailored to the model architecture of Trans- methods, a new trending approach, that make formers, built upon their underlying mathematical use of specific features of the Transformer ar- foundations. These methods make use of specific chitecture and are deemed more promising for features of Transformers, including their layered understanding Transformer-based models. We structure (layers, heads, tokens), the division of la- start by discussing the potential pitfalls and bor between the attention mechanism, feed-forward misleading results model-agnostic approaches layers, and residual streams. These techniques span may produce when interpreting Transformers. Next, we discuss Transformer-specific methods, from those aimed at measuring token-to-token in- including those designed to quantify context- teractions (known as context mixing, Brunner et al., mixing interactions among all input 
- **方法与解释性关系**：该论文主要围绕 `Analysis, Transformer, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：未自动提取到稳定短句，需回看论文中的 Baselines、Comparison 或 Ablation 小节。

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - widespread use of model-agnostic interpretabil- effectiveness in drawing reliable conclusions has
  - Attribution patching outperforms automated circuit

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ity techniques, including gradient-based and therefore been an ongoing matter of debate (Bibal occlusion-based, their shortcomings are becom- et al., 2022). ing increasingly apparent for Transformer in- Recently, a game-changing trend has emerged: terpretation, making the field of interpretability more demanding today. In this tutorial, we will the development of analysis methods that are pre- present Transformer-specific interpretability cisely tailored to the model architecture of Trans- methods, a new trending approach, that make formers, built upon their underlying mathematical use of specific features of the Transformer ar- foundations. These methods make use of specific chitecture and are deemed more promising for features of Transformers, including their layered understanding Transformer-based models. We structure (layers, heads, tokens), the division of la- start by discussing the potential pitfalls and bor between the attention mechanism, feed-forward misleading results model-
- **局限/未来工作线索**：such as probing, occlusion-based, and feature attri- clude with a discussion on current limitations in；et al., 2020) and "Fine-grained Interpretation and limitations?；鈥 Limitations of interpreting raw attention
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Transformer-specific Interpretability”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
