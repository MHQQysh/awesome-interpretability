# 096. Mechanistic Understanding and Mitigation of Language Model Non-Factual Hallucinations

> 逐篇阅读记录：第 96 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Lei Yu, Meng Cao, Jackie CK Cheung, Yue Dong
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-emnlp.466)
- **领域标签**：Mechanistic, Representation, Evaluation, Hallucination, Hidden, Layer, Detect
- **本地 PDF 文本规模**：约 9,144 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Mechanistic为主线，结合论文摘要中的核心设定：State-of-the-art language models (LMs) sometimes generate non-factual hallucinations that misalign with world knowledge.To explore the mechanistic causes of these hallucinations, we create diagnostic datasets with subjec
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3. In the third run, we again provide the model；7 Limitation；2023. Challenges and applications of large language；7) You digest the watermelon seeds
- **引言关键线索**：Language models (LMs) serve as repositories of et al., 2022a; Geva et al., 2023). However, it re- substantial knowledge (Petroni et al., 2019; Jiang mains unclear whether the results of mechanistic in- et al., 2020; Srivastava et al., 2023) through their terpretability on factual predictions can generalize parametric knowledge gained from pre-training. to hallucinations. Specifically, it is unknown which However, they are susceptible to generating 鈥渉allu- model components deviate from normal function- cinations鈥 that contain factual errors. At the level of ing to cause hallucinations. Localizing the source logit predictions, these hallucinations often display of non-factual hallucination in LMs may help us a pattern similar to factual generations. For exam- design targeted and efficient methods to mitigate ple, LMs have been observed to produce seemingly hallucinations without significan
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：trace hallucinations through internal model rep- 2023; Zhang et al., 2023a) treat the LM as a black resentations. We discover two general and box, devising methods based on external features distinct mechanistic causes of hallucinations like predictive uncertainty (Xiao and Wang, 2021; shared across LMs (Llama-2, Pythia, GPT-J): 1) knowledge enrichment hallucinations: in- Varshney et al., 2023) and logical consistency (Co- sufficient subject attribute knowledge in lower hen et al., 2023). These approaches provide little layer MLPs, and 2) answer extraction hallu- insight into the internal mechanisms of factual er- cinations: failure to select the correct object rors and have been shown to be unreliable with attribute in upper layer attention heads. We also often contradictory signals (Turpin et al., 2023). found these two internal mechanistic causes of In contrast, interpretability research, which ex- hallucinations are reflected in ext
- **方法与解释性关系**：该论文主要围绕 `Mechanistic, Representation, Evaluation, Hallucination, Hidden, Layer, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Baseline Methods. We evaluate, MHM against SFT 34.4, MEND 36.7, LM factual errors, Similarly
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - to baselines 1 . heads, feedforward layers) related to knowledge
  - Baseline Methods. We evaluate MHM against SFT 34.4 / 86.0 46.0 / 92.3
  - several baseline methods that have shown promis- MEND 36.7 / 89.2 33.3 / 83.7
  - contrast, other baselines often yield inferior perfor- tinct mechanisms of LM factual errors: insufficient
  - on the other hand, the model predicted objects are Similarly, for in-context learning baseline method,

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：3 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - ple, LMs have been observed to produce seemingly hallucinations without significantly impacting util-
  - fore processing the relation. Similarly, given the the entire vocabulary. In contrast, for a significant
  - retrieve a significant amount of object information et al., 2020; Meng et al., 2022a), which quantifies
  - (i.e., we only keep noises that make the model contribute significantly to enrichment hallucina-
  - the entropy of conditional next-token distribution In practice, we find that LMHM can be combined
  - cinations are significantly less robust under input
  - and demonstrate that it can improve LM factuality per token for each candidate answer, and define a

## 6. Conclusion、局限与可复现性

- **结论段落线索**：effective mitigation results, or achieves a perfor- mance that is comparable to the best mitigation We conducted various interpretability analyses on method. Meanwhile, MHM in most cases pre- non-factual hallucinations made by language mod- serves more than 90% of model performance on els. We show that both lower layer MLPs and upper the specificity evaluation sets, indicating that our layer attention heads in the model factual knowl- mechanistic mitigation of hallucinations does not edge recalling pipeline may operate abnormally compromise LMs鈥 general world knowledge. In during model inference, thereby leading to two dis- contrast, other baselines often yield inferior perfor- tinct mechanisms of LM factual errors: insufficient mance on at least one of the two datasets. In par- knowledge enrichment and ineffective answer ex- ticular, knowledge editing methods such as MEND traction. Leveraging these insights, we proposed an struggles at Truthful QA on which an LM often effective method
- **局限/未来工作线索**：Our study bears several limitations. Firstly, cer- Kim, James R Glass, and Pengcheng He. 2023. Dola:；ticularly in early layers. Future work should con-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Mechanistic Understanding and Mitigation of Language Model Non-Factual Hallucinations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
