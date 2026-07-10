# 180. Distilling Text Style Transfer With Self-Explanation From LLMs

> 逐篇阅读记录：第 180 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Chiyu Zhang, Honglong Cai, Yuezhang Li, Yuexin Wu, Le Hou, Muhammad Abdul-Mageed
- **发表 venue / date**：NAACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.naacl-srw.21)
- **领域标签**：Rationale, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 7,579 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Chiyu Zhang, Honglong Cai, Yuezhang Li, Yuexin Wu, Le Hou, Muhammad Abdul-Mageed.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction promising technique that extracts these reasoning；4 Results；6 Conclusion；8 Ethical Consideration Zhanghao Wu, Hao Zhang, Lianmin Zheng, Siyuan；2022. Training language models to follow instruc- cessing and the 9th International Joint Conference；1. Similarity: To evaluate the similarity be-；2. Transfer Accuracy: To evaluate the effi-；3. Fluency: To access the fluency of the trans-
- **引言关键线索**：skills and enhances accuracy in target tasks. How- TST aims to rephrase a source text s with the de- ever, deploying these enormous LLMs poses com- sired style 蟿 while preserving its core meaning and putational and practical challenges. Recent stud- ensuring fluency of the generated text t (Jin et al., ies (Huang et al., 2022; Wang et al., 2023a; Hsieh 2022). The term 鈥渟tyle" can encompass the per- et al., 2023) have thus turned to offline knowledge sonal characteristics of an author, such as age, and distillation (KD) (Hinton et al., 2015) to condense pragmatic use like formality or toxicity. To develop these reasoning capabilities into a smaller model. TST systems using supervised methods, several Using CoT rationales can also increase distillation human-annotated datasets have emerged (Rao and efficiency with less data (Li et al., 2022; Shridhar Tetreault, 2018). For instance, Rao and
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：els (LLMs) alongside chain-of-thought (CoT) Reid and Zhong (2021) employ an auxiliary style prompting to facilitate TST. CoTeX distills the classifier to steer the transfer direction. Meanwhile, complex rewriting and reasoning capabilities Krishna et al. (2020) and Hallinan et al. (2023b) of LLMs into more streamlined models capable deploy multiple style-specific models to produce of working with both non-parallel and parallel various styles individually. Of late, LLMs have data. Through experimentation across four TST demonstrated exceptional prowess across diverse datasets, CoTeX is shown to surpass traditional supervised fine-tuning and knowledge distil- NLP tasks. Studies like Reif et al. (2022); Pu and lation methods, particularly in low-resource Demberg (2023) have found that extremely large settings. We conduct a comprehensive eval- LMs, with over 100B parameters, are adept at TST uation, comparing CoTeX against current unsu- wit
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The corresponding ther compared, Prompt, Rank, SoTA, This facilitates direct comparisons, Supervised methods include Multi-, Model Comparison. We set, As depicted in Figure, CoTeX-TB fer directions to
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - si and target style 蟿 as input. The corresponding ther compared with (1) Prompt&Rank: a SoTA
  - evaluation sets. This facilitates direct comparisons spearean text. Supervised methods include Multi-
  - 3.2 Model Comparison. We set the maximal input and output sequence
  - text. As depicted in Figure 5, although CoTeX-TB fer directions to enable a clear comparison between

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - main expert feedback for improved formality trans-
  - Work done during internship at Google. ing CoT prompting to improve TST. It identifies
  - TeX (CoTeX-TA) consistently outperforms SFT
  - training data sizes. (3) Our CoTeX-TB outperforms
  - outperforms SFT in most data sizes.
  - CoTeX-TB outperforms SFT-T5 and Distill-T5. son. CoTeX-TB surpasses previous unsupervised
  - modern English, CoTeX-TB exhibits significant su- the detoxification task, our results are compared

## 6. Conclusion、局限与可复现性

- **结论段落线索**：take the quality of the transferred text into account. Table 2: Case study on CoTeX-TB generations. Table 3: Human evaluation protocol and instruction. We adapt the evaluation criteria from Wu et al. (2023) and Wang et al. (2023b). short of CondBERT, which employs additional style-conditional LMs for transfer control. FlanT5- XL, an instruction-tuned LLM, leads in ICL perfor- mance with a BLEU score of 50.13. For translat- ing Shakespearean to modern English, CoTeX-TB Qualitative Study. We now present a qualitative shows marked improvements over both unsuper- study to delve into rewriting rationales generated vised and ICL methods, attributed to the superior by CoTeX-TB. Examples are showcased in Table 2, quality of LLM generations in this specific transfer derived from Test set of Formality (F&R) which task. transfers from informal to formal text. We sort generations by their BLEU scores against gold ref- Increasing Synthetic Data per Source Text. For erences and select random high an
- **局限/未来工作线索**：positive correlation between the student model鈥檚 potential limitations in its ability to understand im-；included the source text, a generated rationale for 7 Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Distilling Text Style Transfer With Self-Explanation From LLMs”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
