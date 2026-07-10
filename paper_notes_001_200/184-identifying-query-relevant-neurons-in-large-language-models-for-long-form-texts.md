# 184. Identifying Query-Relevant Neurons in Large Language Models for Long-Form Texts

> 逐篇阅读记录：第 184 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Lihu Chen, Adam Dejl, Francesca Toni
- **发表 venue / date**：AAAI / 2025/04
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/34529/36684)
- **领域标签**：Attribution, Evaluation, Hidden, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 7,634 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Large Language Models (LLMs) possess vast amounts of knowledge within their parameters, prompting research into methods for locating and editing this knowledge.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction While these studies successfully identify knowledge as-；3 Background；4 Locating Query-Relevant (QR) Neurons in；6 Potential Applications Acknowledgments；2019 Conference of the North American Chapter of the As-；2021. Can Language Models be Biomedical Knowledge
- **引言关键线索**：Large Language Models (LLMs) contain substantial sociations stored within LLMs, three significant questions amounts of knowledge within their neurons (or parameters). remain underexplored: (1) How can we effectively lo- Recent research has focused on identifying and localizing cate query-relevant neurons in contemporary decoder-only these knowledge neurons to gain insights into the infor- LLMs, such as Llama (Touvron et al. 2023) and Mis- mation processing mechanisms of LLMs. Activation-based tral (Jiang et al. 2023), given their large model size and methods (Voita, Ferrando, and Nalmpantis 2023) examine different architectures? (2) How can we address the chal- activation patterns to elucidate the role of neurons in the lenge of long-form text generation, as previous methods reasoning process. However, these methods often struggle have been limited to triplet facts? (3) Are there localiz
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Texts Location Models LLMs Large Language Models (LLMs) possess vast amounts of knowledge within their parameters, prompting research into Knowledge Neuron (2022) 鉁 鉁 鉁 鉁 methods for locating and editing this knowledge. Previous ROME (2022a) 鉁 鉁 鉁 鉁 work has largely focused on locating entity-related (often Knowledge Subnetwork (2023) 鉁 鉁 鉁 鉁 single-token) facts in smaller models. However, several key QRNCA (Ours) 鉁 鉁 鉁 鉁 questions remain unanswered: (1) How can we effectively lo- cate query-relevant neurons in decoder-only LLMs, such as Table 1: Comparison of general-domain knowledge locating Llama and Mistral? (2) How can we address the challenge methods. Here, we do not consider task-specific approaches of long-form (or free-form) text generation? (3) Are there lo- like Language Neuron (Chen et al. 2024b) and Privacy Neu- calized knowledge regions in LLMs? In this study, we intro- ron (Wu et al. 2023). duce Query-Relevant Neuron Clus
- **方法与解释性关系**：该论文主要围绕 `Attribution, Evaluation, Hidden, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：LLMs, Table 1, Comparison of general-domain knowledge, More ferent approach by, Voita, Ferrando, Nalmpantis 2023, Gurnee, Table 4, Comparisons of different knowledge, We compare our, Baselines We compare QRNCA, Random Neuron are randomly, Figure 2a illustrates the, PCR, This indicates that The, Additionally, ICA demic domains and
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - cate query-relevant neurons in decoder-only LLMs, such as Table 1: Comparison of general-domain knowledge locating
  - our method outperforms baseline methods significantly. More ferent approach by employing causal mediation analysis to
  - demonstrate that our proposed method outperforms baseline methods (Voita, Ferrando, and Nalmpantis 2023; Gurnee
  - Table 4: Comparisons of different knowledge locating
  - rons influence the predictions of LLMs. We compare our
  - approach to other baseline methods and include a control

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - our method outperforms baseline methods significantly. More ferent approach by employing causal mediation analysis to
  - subject domains. Finally, we show potential applications of
  - Large Language Models (LLMs) contain substantial sociations stored within LLMs, three significant questions
  - each query-answer fact. The process begins by transforming we show that QRNCA might be useful for knowledge edit-
  - demonstrate that our proposed method outperforms baseline methods (Voita, Ferrando, and Nalmpantis 2023; Gurnee
  - To validate the impact of our identified QR neurons, we our conclusions.
  - ods. Our QRNCA method consistently outperforms other or language-specific neurons on a 2D geographical heatmap.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：replicate the experiments by Dai et al. (2022), updating the values of QR neurons using two methods: given a query and 5.4 Are There Localized Regions in LLMs? the value of f炉il , we either (1) boost the neurons by doubling Given our ability to identify QR neurons for each query, the value fil = 2 脳 f炉il ; or (2) suppress the neuron by making it is intriguing to explore whether LLMs exhibit localized fil = 0. After one operation, we record the PCR on a specific regions for each domain or language, analogous to the func- dataset to show the quality of these neurons. tional localizations in the human brain (Brett, Johnsrude, Table 4 presents the overall performance of various meth- and Owen 2002). To investigate this, we visualize domain- ods. Our QRNCA method consistently outperforms other or language-specific neurons on a 2D geographical heatmap. baselines, evidenced by its higher PCR. This indicates that The width of the heatmap corresponds to the dimension of our identified QR neuron
- **局限/未来工作线索**：gions, we construct two multi-choice QA datasets encom- stages. However, a key limitation of these methods is their；Random Neuron 0.0 0.3 0.2 0.3 nation detection presents a promising line of future work.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Identifying Query-Relevant Neurons in Large Language Models for Long-Form Texts”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
