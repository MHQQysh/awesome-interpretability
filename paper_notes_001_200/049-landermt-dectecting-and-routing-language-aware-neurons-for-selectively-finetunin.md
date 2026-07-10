# 049. LANDeRMT: Dectecting and Routing Language-Aware Neurons for Selectively Finetuning LLMs to Machine Translation

> 逐篇阅读记录：第 49 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Shaolin Zhu, Leiyu Pan, Bo Li, Deyi Xiong
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.656)
- **领域标签**：Analysis, Hidden, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 8,618 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Recent advancements in large language models (LLMs) have shown promising results in multilingual translation even with limited bilingual supervision.The major challenges are catastrophic forgetting and parameter interfer
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Methodology as follows:；1. We first propose a method to analyse which FFN；4 Experiments；5 Analysis；2023. Knowledge transfer in incremental learning for tion for Machine Translation, EAMT 2023, Tampere,
- **引言关键线索**：Section 4.3, we find that full-parameter finetuning Conventional neural machine translation (NMT) of LLMs cannot always improve translation quality usually requires a huge amount of parallel training on all language pairs. Therefore, is it possible to data (Costa-juss脿 et al., 2022; Fedus et al., 2022; design a new finetuning method for LLMs, which Zhu et al., 2023, 2024b). In contrast, multilingual can mitigate the issues of catastrophic forgetting LLMs, e.g., BLOOM (Scao et al., 2022), LLaMA2 and parameter interference during the finetuning (Touvron et al., 2023), in spite of being trained with process of LLMs to multilingual machine transla- 鈥 Equal contribution. tion? * Corresponding author. In multilingual NMT, previous efforts evaluate 1 Regarding catastrophic forgetting and parameter interfer- the importance of model neurons to each language ence, we are specifically addressing is
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：that selectively finetunes LLMs to Machine to use various finetuning methods, such as adapter- Translation with diverse translation training based method (Alves et al., 2023), instruction- data. In LANDeRMT, we evaluate the aware- based tuning method (Li et al., 2023a). However, ness of neurons to MT tasks and catego- these approaches primarily focus on balancing be- rize them into language-general and language- specific neurons. This categorization enables tween the original LLMs and new finetuning trans- selective parameter updates during finetuning, lation data They use only incremental data to ac- mitigating parameter interference and catas- quire new knowledge without considering catas- trophic forgetting issues. For the detected trophic forgetting of knowledge originally captured neurons, we further propose a conditional by LLMs (Liu et al., 2021; Shao and Feng, 2022; awareness-based routing mechanism to dynam- Huang et al., 2023)
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hidden, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：NMT, Xie et al, Patel, FFNG, CAR, Settings and Baselines, LLMs, Baselines We compared our, Comparison with ICL In, Comparison with finetuning baselines, Com- rameters. The LANDeRMT, Compared to the full, Layers method, In comparison to other, LANDeRMT-LS
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - various strong baselines for multiple language ally generally developed for multiple tasks (i.e.,
  - compared to previous strong baselines and tional multilingual NMT (Xie et al., 2021; Patel
  - hG (xt ) = FFNG (CAR(xt ).xt ) (7) 4.2 Settings and Baselines
  - parameters of LLMs. Baselines We compared our approach to the fol-
  - against a set of strong baselines. strations.
  - Comparison with ICL In order to examine the rameters of the selected layers, with each language

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Section 4.3, we find that full-parameter finetuning
  - of LLMs cannot always improve translation quality
  - relevant parameters of LLMs for MT. the target language. The most significant changes
  - Once we find the language-pair-relevant layer, do Self Attention
  - our method achieves significant improvements over From Table 2, we observe that LANDeRMT-LS
  - notable advantage in terms of the number of param- improvement over Layers in en-fr, a 9.64 BLEU
  - eters to be tuned. In comparison to other efficient improvement over LANDeRMT-LS, and a 3.76

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Development Program of Yunnan Province (Grant In this paper, we have presented a novel approach No. 202203AA080004). We would like to thank that not only improves translation quality but also the anonymous reviewers for their insightful com- mitigates the risk of forgetting previous knowledge ments. 12143 Limitations Daixuan Cheng, Shaohan Huang, and Furu Wei. 2023. Adapting large language models via reading compre- Although LANDeRMT is a new approach to fine- hension. CoRR, abs/2309.09530. tune LLMs to enhance the translation ability of LLMs. The finetuning procedure is shorter than Arthur Conmy, Augustine N. Mavor-Parker, Aengus Lynch, Stefan Heimersheim, and Adri脿 Garriga- training LLMs as the amount of data required dur- Alonso. 2023. Towards automated circuit dis- ing the finetuning stage is much smaller than during covery for mechanistic interpretability. CoRR, the training stage. This significantly reduces the abs/2304.14997. cost of training model from scratch but maybe still M
- **局限/未来工作线索**：Limitations Daixuan Cheng, Shaohan Huang, and Furu Wei. 2023.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“LANDeRMT: Dectecting and Routing Language-Aware Neurons for Selectively Finetuning LLMs to Machine Translation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
