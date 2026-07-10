# 165. Calibrating Factual Knowledge in Pretrained Language Models

> 逐篇阅读记录：第 165 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Qingxiu Dong, Damai Dai, Yifan Song, Jingjing Xu, Zhifang Sui, Lei Li
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.findings-emnlp.438)
- **领域标签**：Probing, MLLM, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 7,238 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决多模态大模型视觉-语言证据如何被使用和对齐难以解释的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Previous literature has proved that Pretrained Language Models (PLMs) can store factual knowledge.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Contrastive Knowledge Assessment；3 Knowledge Calibration Our C ALI N ET shares the same architecture；100 EM 85 EM；100 Facts 1000 Facts EM F1 EM F1 EM F1；7 Conclusion Acknowledge
- **引言关键线索**：gle fact has multiple surfaces, we also expect that Recently, Pretrained Language Models (PLMs) the calibrated knowledge should be generalizable have improved performance on various Natural to various text surfaces. Figure 1 illustrates the Language Processing (NLP) tasks (Devlin et al., process of calibration. First, we detect the false 2019; Raffel et al., 2020; Brown et al., 2020). knowledge in PLMs with a Contrastive Knowledge Probing tasks like LAMA (Petroni et al., 2019; Assessing (CKA) method (demonstrated in Fig- Elazar et al., 2021; Jiang et al., 2020) have shown ure 2). Since PLMs make black-box decisions, we that PLMs can store factual knowledge and act as evaluate PLMs via their predictions for simplifica- knowledge bases. Leveraging knowledge in PLMs tion. The key motivation behind CKA is a plain can benefit knowledge-intensive downstream tasks argument that a PLM correctly 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：LI N ET to achieve this goal. To be specific, tion. Knowledge calibration aims to rectifie these wrong we first detect whether PLMs can learn the right knowledge. facts via a contrastive score between right and fake facts. If not, we then use a lightweight method to add and adapt new parameters to spe- In order to deal with the false facts, previous cific factual texts. Experiments on the knowl- work focuses on complementing or modifying edge probing task show the calibration effec- knowledge for a specific downstream task. Yao tiveness and efficiency. In addition, through et al. (2022) proposed retrieving external knowl- closed-book question answering, we find that the calibrated PLM possesses knowledge gen- edge during fine-tuning. Cao et al. (2021b) modi- eralization ability after fine-tuning. Beyond fied specific knowledge after finetuning. However, the calibration performance, we further inves- these methods do not generalize to mu
- **方法与解释性关系**：该论文主要围绕 `Probing, MLLM, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The first step for, The ranking is particularly, We compare the CKA, Kotte ++, Compared with previous work, PLMs, Table 3
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - The first step for calibration is to detect which comparison, we sample multiple erroneous rela-
  - bias. The ranking is particularly susceptible to the limited compared with entities.
  - We compare the CKA score with the rank-based Kotte ++
  - performance on knowledge calibration compared with C. P. and has less negative impacts on the generalization
  - Compared with previous work on similar topics
  - eling ability of PLMs (refer to Table 3) while our knowledge-intensive compared with values in the

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - knowledge. However, we find that facts stored : Fraud Knowledge
  - closed-book question answering, we find that
  - have improved performance on various Natural to various text surfaces. Figure 1 illustrates the
  - two sets do not overlap. We show the example data
  - Results As shown in Table 3, we find that the which sentences are factually correct and which
  - Our C ALI N ET consists of 64 and 256 calibration We show the results for knowledge calibration in
  - calibrated model. The improvement of Top1 pre- calibrated at once. Mitchell et al. (2022) prove that

## 6. Conclusion、局限与可复现性

- **结论段落线索**：In this paper, we reassess the knowledge stored This paper is supported by the National Key in PLMs in a contrastive manner and detect the Research and Development Program of China incorrect knowledge stored in PLMs. We pro- 2020AAA0106700 and NSFC project U19A2065. pose C ALI N ET , which adds new parameters to calibrate the knowledge stored in PLMs at scale References without updating the original model parameters. The knowledge-calibrated PLMs generalize cal- Jonathan Berant, Andrew Chou, Roy Frostig, and Percy Liang. 2013. Semantic parsing on Freebase from ibrated knowledge well and perform better than question-answer pairs. In Proceedings of EMNLP. original PLMs on various downstream tasks like open-domain QA. We further provide neuron-level Zied Bouraoui, Jos茅 Camacho-Collados, and Steven investigations on the calibration mechanism and Schockaert. 2020. Inducing relational knowledge from bert. In Proceedings of AAAI. study how calibration works. Tom B. Brown, Benjamin Mann, Nick 
- **局限/未来工作线索**：To address these limitations, we propose CKA to；In order to delve deeper into the scale limitation Number of Calibration Memory Slots We con-；future work can present more advanced knowledge；Limitations and Future Work Subbiah, Jared Kaplan, Prafulla Dhariwal, Arvind
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Calibrating Factual Knowledge in Pretrained Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
