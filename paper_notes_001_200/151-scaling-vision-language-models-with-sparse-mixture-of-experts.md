# 151. Scaling Vision-Language Models with Sparse Mixture of Experts

> 逐篇阅读记录：第 151 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Sheng Shen, Zhewei Yao, Chunyuan Li, Trevor Darrell, Kurt Keutzer, Yuxiong He
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.758)
- **领域标签**：Evaluation, MLLM, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 14,179 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决多模态大模型视觉-语言证据如何被使用和对齐难以解释的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：The field of natural language processing (NLP) has made significant strides in recent years, particularly in the development of large-scale vision-language models (VLMs).
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related Work For pretraining objectives, multiple cross-modal pre-；0 Train2K；2023. Openflamingo: An open-source framework for Meng Huat Tiong, Junqi Zhao, Weisheng Wang,；2019. ViLBERT: Pretraining task-agnostic visiolin-；2021. Florence: A new foundation model for com-；35 LIMoEsmall/E16
- **引言关键线索**：2022), where the authors proposed a modal-agnostic The ability to understand and generate natural language CLIP-style (Radford et al., 2021) multimodal MoEs ar- from visual information is a critical component of many chitecture, but their focus is mainly on the contrastive real-world applications, including visual question an- pre-training objective and vision-only downstream tasks. swering (VQA), visual reasoning, and multimodal in- There are two limitations in this setting: (1) The increas- formation retrieval. In recent years, the success of deep ing model capacity of MoEs under the the simple con- learning in natural language processing (NLP) has led to trastive objective can easily lead to over-fitting issues. the development of large-scale vision-language models (2) The vision-only benchmarking does not reveal the (VLMs) (Tan and Bansal, 2019; Chen et al., 2020; Li full power of sc
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：ing this challenge is the use of sparsely-gated ikhin et al., 2020; Zoph et al., 2022; Du et al., 2022). mixture-of-experts (MoE) techniques, which These models are motivated by the need to increase divide the model into smaller, specialized sub- model parameters while controlling compute costs. In models that can jointly solve a task. In this addition, these models provide other advantages, includ- paper, we explore the effectiveness of MoE in ing sparsity that can mitigate catastrophic forgetting in scaling vision-language models, demonstrating continual learningg (Collier et al., 2020; Komatsuzaki its potential to achieve state-of-the-art perfor- et al., 2022), and an inductive bias that can enhance mance on a range of benchmarks over dense performance in multitask learningg (Ma et al., 2018; models of equivalent computational cost. Our Kudugunta et al., 2021; Kim et al., 2021b). Overall, the research offers valuable insights into st
- **方法与解释性关系**：该论文主要围绕 `Evaluation, MLLM, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Following BE I T, Bao et al, As shown Table 2, VL-MoE with two
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - fair comparison, we provide details on the amount of with 1k classes. Following BE I T (Bao et al., 2022a) and
  - with a comparably modest architecture size and training As shown Table 2, we compare VL-MoE with two
  - ple and performant baseline for vision and language.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - has made significant strides in recent years, models like Flamingo-80B (Alayrac et al., 2022), BE I T-
  - Our experiments demonstrate that MoE can significantly et al., 2021; Jia et al., 2021; Yuan et al., 2021) separately
  - improve the efficiency and effectiveness of VLMs, en- encodes each modality with different encoders. While
  - In summary, our contributions are as follows: However, this design sacrifices efficiency for improved
  - bers, and scaling either T-FFN or V-FFN alone, for improved scalability and achieving state-of-the-art
  - et al., 2020; Kim et al., 2021a); (2) Generative mod- While many works aim to improve the gating mecha-
  - deemed more informative. Our results show that BPR 2017) for T-FFN鈥檚 MoE and averaged loading loss and

## 6. Conclusion、局限与可复现性

- **结论段落线索**：been scaled in VL-MoE, as reflected in the pretraining plot in Figure 2 (the difference of VLM loss between We conduct ablation studies to analyze the contributions VL-MoE and dense BE I T-3 model is smaller compared of Mixture-of-Experts module used in VL-MoE from to that of MLM and MIM loss). We leave the scale of different perspectives. We evaluate the models on visual the VL-FFN module for future work, considering the reasoning (NLVR2), image-text retrieval (Flickr30k), increasing instability in modal-agnostic MoE architec- image classification (ImageNet-1k) and natural lan- tures demonstrated in LIM O E (Mustafa et al., 2022). guage inference (MNLI-m). 11335 Scaling Strategy NLVR2 Flickr30k ImageNet MNLI-m Avg. T-MoE V-MoE dev test-P TR R@1 IR R@1 Acc@1 Acc [1] 鉁 鉁 67.42 68.21 80.4 61.7 67.2 54.3 66.5 [2] 鉁 鉁 72.42 72.73 83.2 64.7 67.8 58.3 69.9 [3] 鉁 鉁 71.19 72.23 82.9 64.5 69.2 55.2 69.2 [4] 鉁 鉁 72.98 73.34 84.7 65.3 69.0 58.1 70.6 Table 3: Ablation studies of scaling strategies
- **局限/未来工作线索**：swering (VQA), visual reasoning, and multimodal in- There are two limitations in this setting: (1) The increas-；the VL-FFN module for future work, considering the reasoning (NLVR2), image-text retrieval (Flickr30k),
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Scaling Vision-Language Models with Sparse Mixture of Experts”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
