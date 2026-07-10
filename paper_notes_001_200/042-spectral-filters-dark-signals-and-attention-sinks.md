# 042. Spectral Filters, Dark Signals, and Attention Sinks

> 逐篇阅读记录：第 42 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Nicola Cancedda
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.263)
- **领域标签**：Representation, Rationale, Lens, Hidden, LLM, Layer, Explain
- **本地 PDF 文本规模**：约 9,300 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Projecting intermediate representations onto the vocabulary is an increasingly popular interpretation tool for transformer-based LLMs, also known as the logit lens (Nostalgebraist).We propose a quantitative extension to
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 The LLaMa2 models We gain an initial insight into whether dark sig-；4 Spectral filtering the U-dark (E-dark) space by multiplying them by；5 Attention sinks；6 Sink-preserving spectral filters；7 Dark Signals and Attention Bars；2022. Competition-level code generation with alpha-；2023. The truth is in there: Improving reasoning in；V Vocabulary
- **引言关键线索**：Large foundation models dominate the state of the Geva et al., 2020). We extend this approach and art in numerous AI tasks. While we understand how introduce logit spectroscopy, the spectral analysis these models work in terms of elementary opera- of the content of the residual stream and of the pa- tions, and black-box evaluations help characterize rameter matrices interacting with it. Equipped with observable behaviours, we lack a clear understand- this tool, we look at the part of the residual stream ing of the connection between the two. spectrum that is most likely to be neglected by the There is a growing body of work providing in- logit lens: the linear subspace spanned by the right sights into properties of model components, e.g. singular vectors of the unembedding matrix with (Voita et al., 2019; Pimentel et al., 2020; Voita the smallest singular values. Drawing an analogy and T
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：art in numerous AI tasks. While we understand how introduce logit spectroscopy, the spectral analysis these models work in terms of elementary opera- of the content of the residual stream and of the pa- tions, and black-box evaluations help characterize rameter matrices interacting with it. Equipped with observable behaviours, we lack a clear understand- this tool, we look at the part of the residual stream ing of the connection between the two. spectrum that is most likely to be neglected by the There is a growing body of work providing in- logit lens: the linear subspace spanned by the right sights into properties of model components, e.g. singular vectors of the unembedding matrix with (Voita et al., 2019; Pimentel et al., 2020; Voita the smallest singular values. Drawing an analogy and Titov, 2020; Geva et al., 2022; Meng et al., with 鈥渄ark matter鈥 in astrophysics, that interacts 2022; Voita et al., 2023), as well as identifying wit
- **方法与解释性关系**：该论文主要围绕 `Representation, Rationale, Lens, Hidden, LLM, Layer, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We and Wu dark
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - only its double projection onto both the We and Wu dark spaces. (a) shows, for comparison, the effect of adding

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and unembedding matrices into bands. We find tween components onto selected subspaces as defined
  - We find that the negative log-likelihood of pre- In this perspective, the probability distribution of
  - Finally, we find a significant positive correlation be- similar). The SV distribution of the We embedding
  - a mild bottleneck after L3 results in a significant
  - are achieved even when masking a significant num- ceive a large amount of attention, giving rise to
  - memory and communication channel across com- 9 Conclusions and Future work
  - nal to the vocabulary representation could encode LLaMa2 models, and we showed that, as long as

## 6. Conclusion、局限与可复现性

- **结论段落线索**：When filtering MLP layers of LLaMa2 13B, the the combined action of MLPs and attention heads effects of masking L0 and L3 dwarf all others. first erases it and then replaces it with the vec- When filtering the output of the MLP at L3 (Fig. tor that encodes the probability distribution of the 7) the loss remains poor until the last 5% of the model over generation-initial tokens. spectrum is included for both the 桅 spectral fil- We confirmed that dark signals are primarily ters. The fact that the 唯 curve is always above the used to sink attention with an additional experi- random filter indicates that this MLP exerts strong ment (Fig. 10). We apply spectral filters at the exit influence operating in the dark subspace. See Ap- of a layer but, rather than suppressing the filtered pendix C for a discussion of 13B/L0/MLP. 3 Xiao et al. (2023) include the first four tokens in their Figure 17 in App. C shows that MLP compo- operational definition of attention sinks, for achieving effective str
- **局限/未来工作线索**：memory and communication channel across com- 9 Conclusions and Future work
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Spectral Filters, Dark Signals, and Attention Sinks”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
