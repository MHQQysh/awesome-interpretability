# 066. Towards Adaptive Prefix Tuning for Parameter-Efficient Language Model Fine-tuning

> 逐篇阅读记录：第 66 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Zhen-Ru Zhang, Chuanqi Tan, Haiyang Xu, Chengyu Wang, Jun Huang, Songfang Huang
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.acl-short.107)
- **领域标签**：Probing, Representation, Evaluation, Hidden, LLM, Layer, Analyze
- **本地 PDF 文本规模**：约 6,096 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Fine-tuning large pre-trained language models on various downstream tasks with whole parameters is prohibitively expensive.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Works keys and values matrix with original corresponding；4 Experiments also verify our method with DeBERTa-xlarge (He；2018 EMNLP Workshop BlackboxNLP: Analyzing
- **引言关键线索**：adaptive prefix in this work. We propose Adaptive Vanilla fine-tuning strategy usually adjusts all the Prefix Tuning (APT) with an adaptive gate mech- parameters to adapt the pre-trained language model anism at both fine-grained token level and coarse- to downstream tasks. Parameter-efficient learning grained layer level. Specifically, as shown in Fig- (He et al., 2022; Houlsby et al., 2019; Lester et al., ure 1, for fine granularity, APT scores each individ- 2021; Guo et al., 2021; Ben Zaken et al., 2022) is ual prefix token via gated weight assignment. Then, an emerging framework that freezes the pre-trained the scaled weight is utilized to balance the inserted model and only tunes a few number of task-specific task-specific prefix tokens and original input tokens parameters for downstream tasks. For instance, for current layer at coarse-grained level. Prefix tuning (Li and Liang, 2021
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：pseudo tokens) inserted into Transformer lay- where the left is the internal structure of Transformer ers. Based on the observation that the learned with inserted prefixes, and the right is the schematic of syntax and semantics representation varies a lot prefix gate mechanism. at different layers, we argue that the adaptive prefix will be further tailored to each layer than the fixed one, enabling the fine-tuning more representational capacity embedded in each layer effective and efficient. Thus, we propose Adap- are prone to be inconsistent (Jawahar et al., 2019). tive Prefix Tuning (APT) to adjust the prefix It is generally considered that the bottom layers of in terms of both fine-grained token level and coarse-grained layer level with a gate mech- the language model tend to capture concrete and anism. Experiments on the SuperGLUE and shallow phrase-level features, while the top layers NER datasets show the effectiveness of APT. con
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, Evaluation, Hidden, LLM, Layer, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Accordingly, APT with vanilla fine-tuning, Table 4, Comparison between PT-2 and, PT-2, APT on BERT-base, P-Tuning v2, Figure 2, From the comparison between, Elad Ben Zaken, Yoav Goldberg, Shauli Ravfogel, APT Short Papers, Minneapolis, Min-, For a fair comparison, The pre-trained model we, We use open-source datasets
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - 鈯 is the element-wise multiplication. Accordingly, 2020), we compare APT with vanilla fine-tuning
  - Table 4: Comparison between PT-2 and PT-2鈭 , PT-2+ and APT on BERT-base. (PT-2: P-Tuning v2; PT-2鈭 : PT-2
  - shown in Figure 2). From the comparison between Elad Ben Zaken, Yoav Goldberg, and Shauli Ravfogel.
  - shown in the comparison between PT-2+ and APT Short Papers), pages 2924鈥2936, Minneapolis, Min-
  - 100]. For a fair comparison, the prefix length uti- et al., 2013). The pre-trained model we used are
  - We use open-source datasets and do not change datasets for a fair comparison.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1.5 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - 0.7% improvements over P-Tuning v2 on Super-
  - large, APT outperforms P-Tuning v2 by 1.8% on
  - surprisingly outperforms the original implementa- Proceedings of the 60th Annual Meeting of the As-
  - whether the improvement of APT is brought from Tom Kwiatkowski, Michael Collins, and Kristina
  - of Table 4, we observe that APT still outperforms nesota. Association for Computational Linguistics.
  - 5 Conclusion pages 276鈥286, Florence, Italy. Association for Com-
  - level gate mechanism which achieves an improve- the North American Chapter of the Association for

## 6. Conclusion、局限与可复现性

- **结论段落线索**：What is prefix weight distribution learned by To verify the effectiveness of adaptive prefix under APT? The gate mechanism for prefix serves as the proposed architecture, we wonder if the learned the key strategy of the proposed APT, where the ratio at each layer can be directly transferred to learned prefix weight distribution turns out to be a P-Tuning v2. Taking the gate as a probing indica- critical point. Figure 2 illustrates the gate weights tor, we reset the prefix length of P-Tuning v2 from of the pseudo prefix token for COPA and CoNLL04, fixed to variable in different layers based on the ob- 1242 servation of the learned ratio (e.g. the distribution References shown in Figure 2). From the comparison between Elad Ben Zaken, Yoav Goldberg, and Shauli Ravfogel. PT-2 and PT-2鈭 in Table 4, we demonstrate that 2022. BitFit: Simple parameter-efficient fine-tuning the variable prefix with less trainable parameters for transformer-based masked language-models. In surprisingly outperfor
- **局限/未来工作线索**：Limitations Parameter-efficient transfer learning with diff prun-；from certain limitations, i.e. we adapt APT on the 11th International Joint Conference on Natural Lan-；3 A1. Did you describe the limitations of your work?
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Towards Adaptive Prefix Tuning for Parameter-Efficient Language Model Fine-tuning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
