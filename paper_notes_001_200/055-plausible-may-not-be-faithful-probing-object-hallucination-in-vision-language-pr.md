# 055. Plausible May Not Be Faithful: Probing Object Hallucination in Vision-Language Pre-training

> 逐篇阅读记录：第 55 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Wenliang Dai, Zihan Liu, Ziwei Ji, Dan Su, Pascale Fung
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.eacl-main.156)
- **领域标签**：Probing, Evaluation, MLLM, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 9,491 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Large-scale vision-language pre-trained (VLP) models are prone to hallucinate non-existent visual objects when generating text based on visual information.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related Work duced to additionally handle multimodal genera-；3 Evaluation Setup in which 82K, 5K, and 5K images are in the train,；6 Object Masked Language Modeling lationship between objects, which is a common；7 Conclusion
- **引言关键线索**：and expand the CHAIR (Caption Hallucination As- Thanks to the advancement of large pre-trained sessment with Image Relevance) metric proposed Language Models (LMs) and Vision-Language by Rohrbach et al. (2018). Results show that these Pre-training (VLP) methods, models are able to models still hallucinate frequently with 鈭10% of achieve surprisingly good performance in vision- the generated sentences containing at least one hal- conditioned text generation, e.g., image captioning. lucinated object. This problem becomes much However, large LMs are found to generate unfaith- severer when generating sentences given out-of- ful or nonsensical texts given the source input (Ji domain images. Furthermore, we discover that the et al., 2022), which is called hallucination. The hal- widely used optimization method SCST (Rennie lucination problem is also inherited by VLP mod- et al., 2017) could le
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：achieve surprisingly good performance in vision- the generated sentences containing at least one hal- conditioned text generation, e.g., image captioning. lucinated object. This problem becomes much However, large LMs are found to generate unfaith- severer when generating sentences given out-of- ful or nonsensical texts given the source input (Ji domain images. Furthermore, we discover that the et al., 2022), which is called hallucination. The hal- widely used optimization method SCST (Rennie lucination problem is also inherited by VLP mod- et al., 2017) could lead to more hallucination, even els (Alayrac et al., 2022), as they are still LMs that if it improves standard metrics like CIDEr (Vedan- can understand visual information. VLP models of- tam et al., 2015). ten generate fluent and seem appropriate sentences For our second question, to investigate how if we only see the text, but wrong when taking the different types of image enco
- **方法与解释性关系**：该论文主要围绕 `Probing, Evaluation, MLLM, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：VLP methods, Figure 1, Comparison of image captioning, Although SCST can sig-, Appendix A, Table 3, Comparison of the effects, VLP ob-, Based on the best, ViT-L, ITCLate, Yao et al, As Figure 2, Comparison of ground truth, Kalantidis et al, Figure 3, Comparison of generated captions
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - VLP methods, the performance of image caption- Figure 1: Comparison of image captioning examples
  - need of another baseline. Although SCST can sig- plementation details are described in Appendix A.
  - Table 3: Comparison of the effects of different VLP ob-
  - best performance in terms of avoiding object hal- Based on the best performing ViT-L/14 baseline,
  - tion (ITCLate ) proposed by Yao et al. (2022). As Figure 2: Comparison of ground truth captions in
  - fective on representation learning (Kalantidis et al., Figure 3: Comparison of generated captions with or

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Surprisingly, we find that patch-based features VLP works yet.
  - lucination. Results show that it reduces object hallucination? and 4) how to mitigate object hallu-
  - Language Models (LMs) and Vision-Language by Rohrbach et al. (2018). Results show that these
  - els (Alayrac et al., 2022), as they are still LMs that if it improves standard metrics like CIDEr (Vedan-
  - ilar images and texts, results show that they do data refinement with self-training to improve the
  - improve standard metrics may reflect in worse 2.2 Vision-Language Pre-training
  - perimental results show that it reduces object Shen et al., 2022) are trained to perform multi-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：gate object hallucination by improving object-level image-text alignment. It is named Object Masked This paper systematically studies the objection hal- Language Modeling (ObjMLM). As shown in Fig- lucination phenomenon in VLP models, which is ure 4, ObjMLM can be seen as a variant of the a severe problem but neglected in contemporary MLM loss by masking all the objects in the text VLP works. We find that recent large VLP models that appear in the image. For each sentence, we still hallucinate frequently. Moreover, the widely mask the object words and phrases as defined in used SCST method harms the faithfulness of gen- the object category lists of both COCO and No- erated sentences in image captioning, even if it Caps by performing exact matching. Similar to the improves previous standard metrics. Furthermore, whole word masking (Cui et al., 2021), we conduct we discover that image encoding matters and the whole object masking so that there will be only one patch-based input with smal
- **局限/未来工作线索**：coding in VLP influence hallucination, includ- limitations and risks caused by object hallucination,；could be valuable for future work to build lize image-text pairs crawled from the web. In the；Limitations Jingjing Liu. 2020. Uniter: Universal image-text；for future work. Another limitation is that for the
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Plausible May Not Be Faithful: Probing Object Hallucination in Vision-Language Pre-training”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
