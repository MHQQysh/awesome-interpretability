# 070. EfficientVLM: Fast and Accurate Vision-Language Models via Knowledge Distillation and Modal-adaptive Pruning

> 逐篇阅读记录：第 70 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Tiannan Wang, Wangchunshu Zhou, Yan Zeng, Xinsong Zhang
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-acl.873)
- **领域标签**：Analysis, MLLM, Hidden, Layer, Control
- **本地 PDF 文本规模**：约 10,052 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决多模态大模型视觉-语言证据如何被使用和对齐难以解释的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Pre-trained vision-language models (VLMs) have achieved impressive results in a range of vision-language tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 XL Xh；62 EfficientVLM 112 EfficientVLM；2018. Bottom-up and top-down attention for image；2017. Visual genome: Connecting language and vi- Yongfei Liu, Chenfei Wu, Shao-yen Tseng, Vasudev；2011. Im2text: Describing images using 1 million Meeting of the Association for Computational Lin-；2011. Proceedings of a meeting held 12-14 December；2019 Fifth Workshop on Energy Efficient Machine
- **引言关键线索**：Inspired by the success of large pre-trained lan- Pre-trained vision-language models (VLMs) guage models (Devlin et al., 2019; Radford et al., have achieved impressive results in a range 2018) in the field of natural language processing of vision-language tasks. However, popular (NLP), recent studies (Su et al., 2019; Li et al., VLMs usually consist of hundreds of millions 2020a; Radford et al., 2021a; Kim et al., 2021; of parameters which brings challenges for fine- Li et al., 2021b) in vision-language pretraining tuning and deployment in real-world applica- tions due to space, memory, and latency con- (VLP) have advanced the state-of-the-art on vari- straints. In this work, we introduce a distill- ous vision-language tasks such as image captioning, ing then pruning framework to compress large visual question answering, and image-text retrieval. vision-language models into smaller, fast
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：vision-language models into smaller, faster, and However, in both NLP and vision-language do- more accurate ones. We first shrink the size of mains, large Transformer-based pre-trained mod- a pre-trained large VLM and apply knowledge els often consist of hundreds of millions, if not distillation in the vision-language pre-training stage to obtain a task-agnostic compact VLM. billions, of parameters, bringing various practi- Then we propose a modal-adaptive pruning al- cal challenges for deployment. As summarized gorithm to automatically infer the importance in Schwartz et al. (2020a) and Xu et al. (2021d), of vision and language modalities for differ- large pre-trained models require large amounts of ent downstream tasks and adaptively remove space (in terms of GPU memory and disk storage) redundant structures and neurons in different and heavy computing for fine-tuning and inference, encoders with controllable target sparsity. which is
- **方法与解释性关系**：该论文主要围绕 `Analysis, MLLM, Hidden, Layer, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Then for ITR- 1, In contrast, Baselines, X-VLMsmall in our comparison, Datasets and Tasks, To better illustrate our, Table 2 have cleaned, Specifically, X-VLMclip 5, EfficientVLM with other efficient, We can see that, EfficientVLM substan-, OSCARB baseline 64 95, X-VLM 114 95, X-VLM, EfficientVLM
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - consider a baseline that prunes 30% parameters during training as real numbers in the range [0,
  - out of each encoder as the baseline. Then for ITR- 1]. In contrast, during inference, all the variables
  - knowledge distillation. 4.1 Baselines
  - et al., 2021) and X-VLMsmall in our comparison. 4.2 Datasets and Tasks
  - To better illustrate our comparison, Table 2 have cleaned the pre-training datasets to avoid data
  - for better comparison. Specifically, X-VLMclip 5

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X, 22.1
 X, 4          X, 95%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - 2.2脳. Experimental results show that despite being niques include knowledge distillation (Hinton et al.,
  - achieves a large absolute improvement over Distil- 2019; Wang et al., 2020b; Zhou et al., 2022a; Xu
  - significantly impact performance while pruning the n denotes the number of blocks. By introducing
  - The results are shown in Table 4. We find that We mainly compare EfficientVLM with two base-
  - stantial improvements, demonstrating the effective- tection model and a compact Transformers-based
  - length, we show the input image token length and ate X-VLM on MSCOCO datasets. We adopt the
  - tially outperforms all compared models by a large

## 6. Conclusion、局限与可复现性

- **结论段落线索**：performs manually tuned sparsity. We also find that EfficientVLM performs similarly to the KD-only We introduce EfficientVLM, a fast and accurate variant. These results confirm the effectiveness of vision-language model trained with a distilling modal-adaptive pruning. We also find that pruning then pruning framework. Empirical results show without distillation results in worse results, demon- that EfficientVLM retains 98.4% performance of strating the necessity of knowledge distillation dur- the base-size teacher model while only preserving 13906 44.3% parameters and achieving a speed-up ratio representation learning. In European conference on of 2.2脳. EfficientVLM also achieves a large ab- computer vision, pages 104鈥120. Springer. solute improvement over previous efficient VLMs, Michael Denkowski and Alon Lavie. 2014. Meteor demonstrating a large potential towards lightweight universal: Language specific translation evaluation VLMs. for any target language. In Proceedings of the nint
- **局限/未来工作线索**：3 A1. Did you describe the limitations of your work?
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“EfficientVLM: Fast and Accurate Vision-Language Models via Knowledge Distillation and Modal-adaptive Pruning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
