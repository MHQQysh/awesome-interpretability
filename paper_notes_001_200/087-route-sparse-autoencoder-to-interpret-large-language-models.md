# 087. Route Sparse Autoencoder to Interpret Large Language Models

> 逐篇阅读记录：第 87 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Wei Shi, Sihang Li, Tao Liang, Mingyang Wan, Guojun Ma, Xiang Wang, Xiangnan He
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.346)
- **领域标签**：SAE, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 9,442 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型内部特征叠加、神经元语义不纯导致难以解释的问题。。方法上以SAE为主线，通过表征分析、因果干预或解释评估来连接内部机制与外部行为。
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction a sparse, high-dimensional feature space, which is；2 Methodology instantiated as a TopK SAE due to its superior per-；4 Conclusion；2023. Finding neurons in a haystack: Case studies Sparse feature circuits: Discovering and editing in-
- **引言关键线索**：subsequently reconstructed by a decoder. This pro- Mechanistic interpretability of large language mod- cess reverses the effects of superposition (Elhage els (LLMs) seeks to understand and intervene in et al., 2022a) by extracting features that are sparse, the internal process of information propagation linear, and decomposable. and reasoning, to further improve trust and safety However, the activation strength of features in (Elhage et al., 2022b; Gurnee et al., 2023; Wang this feature space exhibits distinct distribution pat- et al., 2023). Sparse autoencoders (SAEs) identify terns across layers1 (Yun et al., 2021). As shown in causally relevant and interpretable monosemantic Figure 1, low-level features, which are associated features in LLMs, offering a promising solution for with disambiguating word-level polysemy, tend to mechanistic interpretability (Bricken et al., 2023). exhibit 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：two features extracted by Topk SAE in pythia-160m. with a shared SAE to efficiently extract features The low-level feature (visual media terms) exhibits high from multiple layers. It dynamically assigns activation in early layers that gradually decreases in weights to activations from different layers, deeper layers. In contrast, the high-level feature (tempo- incurring minimal parameter overhead while ral expressions) shows increasing activation with depth, achieving high interpretability and flexibility peaking in the later layers. for targeted feature manipulation. We evaluate RouteSAE through extensive experiments on Llama-3.2-1B-Instruct. Specifically, under the same sparsity constraint of 64, RouteSAE ex- Therefore, SAE and its variants (Huben et al., 2024; tracts 22.5% more features than baseline SAEs Rajamanoharan et al., 2024a; Gao et al., 2024; Raja- while achieving a 22.3% higher interpretabil- manoharan et al., 2024b) have b
- **方法与解释性关系**：该论文主要围绕 `SAE, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：SAEs Rajamanoharan et al, Gao et al, Raja-, Figure 2, Comparison of vanilla single-layer, SAE, Crosscoder, RouteSAE, Most existing SAEs belong, This ensures consis- of, Llama-3, Instruct, For baseline SAEs, Baselines, We benchmark RouteSAE against, The random, ReLU SAE, Huben routing baseline yields
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tracts 22.5% more features than baseline SAEs Rajamanoharan et al., 2024a; Gao et al., 2024; Raja-
  - Figure 2: Comparison of vanilla single-layer SAE, Crosscoder, and RouteSAE. Most existing SAEs belong to
  - tions from all routing layers. This ensures consis- of the Llama-3.2-1B-Instruct. For baseline SAEs,
  - Baselines. We benchmark RouteSAE against put x even at high sparsity levels. The random
  - leading baselines, including ReLU SAE (Huben routing baseline yields higher KL divergence than
  - 2024). Moreover, we compare with a random set- Notably, RouteSAE achieves the best perfor-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：8x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and reasoning, to further improve trust and safety However, the activation strength of features in
  - This distribution disparity presents a significant to dynamically extract multi-layer features in an ef-
  - features from the hidden state of a single layer, significantly reduces the number of parameters
  - tional SAEs. This significantly increases computa- feature numbers, and interpretation score. The
  - (2) Uncontrollable interventions: Crosscoder鈥檚 significantly improves the interpretability. At an
  - them, making it impractical to precisely identify a 22.3% improvement in interpretation scores.
  - the first 5% of training steps. (2) Stable phase. identical, yet both exhibit a significant gap com-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Acknowledgments In this paper, we introduce Route Sparse Autoen- coder (RouteSAE), a new framework designed to This research is supported by the National Science enhance the mechanistic interpretability of LLMs and Technology Major Project (2023ZD0121102). by efficiently extracting features from multiple lay- This research was also supported by the advanced ers. Through the integration of a dynamic rout- computing resources provided by the Supercom- ing mechanism, RouteSAE enables the assignment puting Center of the USTC. of layer-specific weights to each routing layer, achieving a fine-grained, flexible, and scalable ap- proach to feature extraction. Extensive experiments References demonstrate that RouteSAE significantly outper- Abien Fred Agarap. 2019. Deep learning using rectified forms traditional SAEs, with a 22.5% increase in linear units (relu). Preprint, arXiv:1803.08375. the number of interpretable features and a 22.3% Trenton Bricken, Adly Templeton, Joshua Batson, improveme
- **局限/未来工作线索**：an alternative to address this limitation, which sep- and reconstruction within a shared SAE, Route-；Crosscoder has two critical limitations: (1) Lim- for tasks requiring robust and interpretable manip-；overcome this limitation, RouteSAE incorporates a；evaluate the reconstruction quality using Kullback- that it has two limitations: (1) Retaining only the
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Route Sparse Autoencoder to Interpret Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
