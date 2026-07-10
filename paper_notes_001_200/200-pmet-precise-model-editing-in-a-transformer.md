# 200. PMET: Precise Model Editing in a Transformer

> 逐篇阅读记录：第 200 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xiaopeng Li, Shasha Li, Shezheng Song, Jing Yang, Jun Ma, Jie Yu
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/29818/31420)
- **领域标签**：Representation, Evaluation, Hallucination, Hidden, Layer, Control
- **本地 PDF 文本规模**：约 7,491 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Model editing techniques modify a minor proportion of knowledge in Large Language Models (LLMs) at a relatively low cost, which have demonstrated notable success.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：100 Editor Efficacy Generalization Specificity；2. The results demonstrate that PMET outperforms existing that additionally updating MHSA weights can slightly im-；2023. Dissecting Recall of Factual Associations in Auto-
- **引言关键线索**：editing performance. In detail, ROME and MEMIT view Large language models (LLMs), as an emerging form of FFN as key-value memories (Geva et al. 2021), where the knowledge base (Petroni et al. 2019; Heinzerling and Inui hidden states before and after passing through FFN weight 2021; Cao et al. 2021), are extensively employed world- W can be considered as keys k and values v, satisfying wide, primarily addressing queries through knowledge re- W k = v. ROME and MEMIT extract TC (Transformer call. Nonetheless, these models are often criticized for fur- Component, namely MHSA and FFN) hidden states as keys nishing erroneous information (Ji et al. 2023; Zhao et al. and optimizes TL (Transformer Layer) hidden states as val- 2023). The cost of fine-tuning or training from scratch to ues, ultimately obtaining the desired weights by solving correct the minor proportion of erroneous knowledge is fr
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：values of key-value memories of the Feed-Forward Network separately based on efficacy and generalization, while the (FFN). They usually optimize the TL hidden states to mem- preservation of irrelevant edited-knowledge is measured by orize target knowledge and use it to update the weights of specificity (also known as locality) (Mitchell et al. 2022a). the FFN in LLMs. However, the information flow of TL hid- For the purpose of uniformity, we collectively refer to ef- den states comes from three parts: Multi-Head Self-Attention ficacy and generalization as reliability. Additionally, two (MHSA), FFN, and residual connections. Existing methods metrics are employed to evaluate the generative capacity neglect the fact that the TL hidden states contains information of the post-edited model: fluency and consistency (Meng not specifically required for FFN. Consequently, the perfor- et al. 2022a). Model editing methods can be classified into man
- **方法与解释性关系**：该论文主要围绕 `Representation, Evaluation, Hallucination, Hidden, Layer, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Figure 1, Comparison between PMET and, Transformer layer, Existing optimization-based methods, C0 has already been, Baselines and Datasets, W0 K1, NeoX, Our baseline methods include, PMET outperforms all baselines, Figure 3, The editing performance of, PMET and baselines mizes, TC hidden states of, FFN
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Figure 1: Comparison between PMET and existing methods in a Transformer layer. (a) Existing optimization-based methods
  - representations to vocabulary tokens, we compare the dif-
  - memorized keys C0 has already been obtained through sam- Baselines and Datasets
  - original model to obtain W0 K1 , we then need the sets of NeoX (20B). Our baseline methods include the improved
  - culating the values of all the target knowledge that need to ficity, PMET outperforms all baselines in all other metrics.
  - Figure 3: The editing performance of PMET and baselines mizes TC hidden states of FFN (i.e., 未ia is removed) for

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - (Meng et al. 2022b) further improves ROME, enabling mass
  - accurate target knowledge representations and compromis- age improvement. Furthermore, our series of ablation exper-
  - of these methods deteriorate significantly. This challenge is given by:
  - upon the conclusion that FFN are key-value memories (Geva
  - original model to obtain W0 K1 , we then need the sets of NeoX (20B). Our baseline methods include the improved
  - culating the values of all the target knowledge that need to ficity, PMET outperforms all baselines in all other metrics.
  - To edit multiple layers of the model, we need to spread factual edits. The results show that PMET outperforms ex-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：et al. 2021) and update FFN weights by optimizing the TL Here, WOl MHSA and WOl FFN are the output weights of the hidden states to memorize target knowledge. While these MHSA and FFN at the l-th layer, respectively, and 蟽 rep- methods have achieved some success in model editing, they resents the non-linear activation function. We have omitted confuse the information flow among the MHSA, FFN, and bias terms for simplicity. residual connections, leading to a non-accurate updates on Model Editing Problem Previous researches on model FFN weights. In contrast to these methods, PMET simulta- editing have been limited to defining the problem solely neously optimizes the TC hidden states of MHSA and FFN based on editing the triples (i.e., subject-relation-object) to memorize target knowledge, while only use the optimized themselves (Meng et al. 2022a; Mitchell et al. 2022b), over- TC hidden states of FFN, facilitating precise updates of FFN looking the knowledge contained within the triples. C
- **局限/未来工作线索**：phenomenon to the limitations imposed by the parameter 鈥 We propose PMET, which leverages the general knowl-；making it more likely to harm specificity. Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“PMET: Precise Model Editing in a Transformer”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
