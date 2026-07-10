# 053. Neurons in Large Language Models: Dead, N-gram, Positional

> 逐篇阅读记录：第 53 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Elena Voita, Javier Ferrando, Christoforos Nalmpantis
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.75)
- **领域标签**：Analysis, Hidden, LLM, Layer, Detect
- **本地 PDF 文本规模**：约 8,530 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：We analyze a family of large language models in such a lightweight manner that can be done on a single GPU.Specifically, we focus on the OPT family of models ranging from 125m to 66b parameters and rely only on whether a
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Dead Neurons；2016. Residual networks behave like ensembles of；2019 Conference on Empirical Methods in Natu-
- **引言关键线索**：layers are dead. At the same time, many of the alive The range of capabilities of language models ex- neurons in this early network part are reserved for pands with scale and at larger scales models be- discrete features and act as indicator functions for come so strong and versatile that a single model can tokens and n-grams: they activate if and only if the be integrated into various applications and decision- input is a certain token or an n-gram. The role of making processes (Brown et al., 2020; Kaplan et al., the updates coming from these token detectors to 2020; Wei et al., 2022; Ouyang et al., 2022; Ope- the residual stream is also surprising: at the same nAI, 2023; Anil et al., 2023). This increases inter- 1 est and importance of understanding the internal Since OPT models have the ReLU activation function, the notion of 鈥渁ctivated鈥 or 鈥渘ot activated鈥 is trivial and means 鈭 Work 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：explicit modular structure that is aimed to be in- rent beliefs in the community. Specifically, (1) in- terpretable by construction (Andreas et al. (2016); formation can be explicitly removed (rather than Hu et al. (2018); Kirsch et al. (2018); Khot et al. added) from the residual stream, (2) positional neu- (2021), to name a few). Intuitively, choosing ReLU rons question the key-value memory view of FFNs, activation function as done in the OPT models can (3) we explain how LMs trained without positional be seen as having the same motivation as devel- information still encode position, (4) we show that oping sparse softmax variants: exact zeros in the minor architecture changes can significantly influ- model are inherently interpretable. ence interpretability, among others. On top of that, our analysis can easily be extended to other models:
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hidden, LLM, Layer, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Figure 10, Positional neurons in 125m
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Figure 10: Positional neurons in 125m models: baseline vs model without positional encoding.
  - tional patterns learned by the baseline 125m model

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - FFN neuron is activated or not. First, we find models up to 66b parameters and deliberately keep
  - so (or not at all) on textual data. We find that
  - position range indicators while larger models First, we find that in the first half of the network,
  - Finally, we find that some neurons are responsi-
  - To sum up, we find neurons that: activation frequency (Figure 1).
  - 鈥 are 鈥渄ead鈥, i.e. never activate on a large di- Many neurons are 鈥渄ead鈥. First, we find that
  - they are 鈥渃ompletely dead鈥) or (ii) they correspond by the number of covering them n-grams (we show

## 6. Conclusion、局限与可复现性

- **结论段落线索**：鈥渃oncept-to-neuron鈥 ratio being much smaller in the encoded concepts. On the contrary: it further sup- early layers than in the higher layers. Intuitively, ports the hypothesis that models assign dedicated the model has to represent sets of encoded in a neurons to specific concepts. layer concepts by 鈥渟preading鈥 them across avail- able neurons. In the early layers, encoded concepts 4 N-gram-Detecting Neurons are largely shallow and are likely to be discrete Now, let us look more closely into the patterns (e.g., lexical) while at the higher layers, networks encoded in the lower model halves and try to un- learn high-level semantics and reasoning (Peters derstand the nature of the observed above sparsity. et al., 2018; Liu et al., 2019; Jawahar et al., 2019; Specifically, we analyze how neuron activations de- Tenney et al., 2019; Geva et al., 2021). Since the pend on an input n-gram. For each input text with number of possible shallow patterns is not large tokens x1 , x2 , ..., xS , we r
- **局限/未来工作线索**：Limitations Shelby, Ambrose Slone, Daniel Smilkov, David R.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Neurons in Large Language Models: Dead, N-gram, Positional”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
