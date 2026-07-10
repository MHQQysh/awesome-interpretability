# 190. Attention Guided CAM: Visual Explanations of Vision Transformer Guided by Self-Attention

> 逐篇阅读记录：第 190 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Saebom Leem, Hyunseok Seo
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/28077/28160)
- **领域标签**：Attribution, Rationale, MLLM, Behavior, Detect
- **本地 PDF 文本规模**：约 6,797 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决多模态大模型视觉-语言证据如何被使用和对齐难以解释的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Vision Transformer(ViT) is one of the most widely used models in the computer vision field with its great performance on various tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2012. LeRF represents the area under the LeRF curve and
- **引言关键线索**：Transformer-based models (Vaswani et al. 2017; Devlin and Zuidema 2020) and Layer-wise Relevance Propagation et al. 2018; Liu et al. 2019; Radford et al. 2018) is a widely (LRP)-based method (Chefer, Gur, and Wolf 2021). used architecture in various NLP tasks due to its superior Attention Rollout is a method developed for ViT and aims performance. Vision Transformer (ViT) (Dosovitskiy et al. to provide a concise aggregation of the overall attention by 2020) is a modified Transformer that adopts the architecture using the resulting matrix of self-attention operation. Al- of BERT (Devlin et al. 2018), but is applicable to images by though it considers the core component of ViT architec- replacing its basic unit of operation with image patches. As ture, it assumes a linear combination of attention and over- a Transformer-based model, ViT applies the self-attention looks the influence of the
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：essary, but these methods employed in CNN-based models mentation (Ranftl, Bochkovskiy, and Koltun 2021; Zheng are still not available in ViT due to its unique structure. In this et al. 2021). Demonstrating its high versatility and decent work, we propose an attention-guided visualization method performance, especially in large-scale image data, it is now applied to ViT that provides a high-level semantic explana- considered as a practical alternative to Convolutional Neu- tion for its decision. Our method selectively aggregates the ral Network (CNN) (He et al. 2016; LeCun et al. 1989; Si- gradients directly propagated from the classification output monyan and Zisserman 2014; Szegedy et al. 2015) which to each self-attention, collecting the contribution of image has dominated the computer vision field for the past decade. features extracted from each location of the input image. These gradients are additionally guided by the normalized D
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, MLLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 1, Localization performance comparison on, ImageNet, Table 2, Pascal Pixel Perturbation. The, Table 3, CUB 200. and reliability
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - is demonstrated in the perturbation comparison test. token and the self-attention mechanism, makes it compli-
  - comparison of our method with previous leading methods.
  - Table 1: Localization performance comparison on ImageNet
  - Table 2: Localization performance comparison on Pascal Pixel Perturbation. The result of the pixel perturbation test
  - Table 3: Localization performance comparison on CUB 200. and reliability of the explanations that our method provides.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - sult, our method outperforms the previous leading explain-
  - of the Transformer over other models: it significantly re- sion. On the other hand, the LRP-based method applied to
  - analysis that aims to improve localization performance by the self-attention scores to final visualization heatmaps of
  - 鈥 Our method outperforms the previous leading methods (Binder et al. 2016b). Finally, Chefer et al., (Chefer, Gur,
  - localization. We also demonstrate the improved reliabil- to ViT by proposing the method to apply LRP to the GELU
  - with improved localization performance, adding channel- contains the high-level image features elicited through the
  - interpolated to have the same size as the input image and each dataset demonstrate that our method greatly improves

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Attention Rollout LRP-based Ours planation of ViT that consistently shows an outstanding per- pixel accuracy 0.5592 0.5750 0.7521 formance in the weakly-supervised object detection task IoU 0.1645 0.1574 0.5335 compared to the Attention rollout and LRP-based method. dice (F1) 0.2431 0.2348 0.6646 Our method shows significant improvements in terms of precision 0.5802 0.7651 0.8167 recall 0.2115 0.1716 0.6647 pixel accuracy, IoU, recall, and dice coefficient, while it still maintains an acceptable level of precision. Table 2: Localization performance comparison on Pascal Pixel Perturbation. The result of the pixel perturbation test VOC 2012. is presented in Table 4. LeRF and MoRF represent the areas under the prediction probability score curve when remov- Attention Rollout LRP-based Ours ing the least relevant pixels first and the most relevant pix- els first, respectively. The ABPC is the area between these pixel accuracy 0.7273 0.7039 0.8351 IoU 0.3097 0.1997 0.5836 two curves, which i
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Attention Guided CAM: Visual Explanations of Vision Transformer Guided by Self-Attention”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
