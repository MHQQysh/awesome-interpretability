# 082. Domain Knowledge Transferring for Pre-trained Language Model via Calibrated Activation Boundary Distillation

> 逐篇阅读记录：第 82 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Dongha Choi, Hongseok Choi, Hyunju Lee
- **发表 venue / date**：ACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.acl-long.116)
- **领域标签**：Analysis, Hidden, LLM, Activation, Analyze
- **本地 PDF 文本规模**：约 7,855 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Since the development and wide use of pretrained language models (PLMs), several approaches have been applied to boost their performance on downstream tasks in specific domains, such as biomedical or scientific domains.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 DoKTra framework；4 Experiments；2095. Martin Krallinger, Obdulia Rabal, Saber A Akhondi,
- **引言关键线索**：known knowledge transfer method that is primarily Recently, transformer (Vaswani et al., 2017)-based used for model compression. The knowledge from language models have been successfully applied in a larger and more effective teacher model is dis- the field of natural language processing (NLP). In tilled to a smaller student model by encouraging it particular, the two-stage approach of 鈥減re-training to mimic the teacher characteristics, such as soft and fine-tuning,鈥 such as BERT (Devlin et al., probabilities (Hinton et al., 2015) or hidden repre- 2019), has become the standard for NLP applica- sentations (Kim et al., 2018; Sun et al., 2019). tions. Generally, a transformer-based model is pre- In this study, we propose a domain knowledge trained with a large amount of text data in an unsu- transfer (DoKTra) framework for an advanced 鈭 Hyunju Lee is the corresponding author. PLM via calib
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：formance on downstream tasks in specific do- tinue to emerge, including ALBERT (Lan et al., mains, such as biomedical or scientific domains. 2019) or RoBERTa (Liu et al., 2019). Additional pre-training with in-domain texts However, these models must be further improved is the most common approach for providing for tasks requiring domain knowledge, such as domain-specific knowledge to PLMs. How- those in the biomedical or financial domains, as ever, these pre-training methods require consid- erable in-domain data and training resources the pre-training data usually consist of general do- and a longer training time. Moreover, the train- main text (e.g., Wikipedia). Additional pre-training ing must be re-performed whenever a new PLM with in-domain text has been proposed to provide emerges. In this study, we propose a domain the PLMs with domain-specific knowledge. For ex- knowledge transferring (DoKTra) framework ample, in the biomedical d
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hidden, LLM, Activation, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：PLMs by applying knowledge, In have been successfully, Figure 1, Comparison between, The comparison between our, Table 3, Performance comparison between existing, Table 4, Performance comparison between TAPT, Performance comparisons followed the, TAPT except, Lewis et al, RoBERTa-large DoKTra for a, The possible maxi-, As mentioned be- The, As both TAPT and, DoK- and distillation steps, The comparison of TAPT
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - PLMs by applying knowledge distillation. In have been successfully used as strong baselines
  - Figure 1: Comparison between (a) an existing domain student models that retain most of the perfor-
  - The comparison between our framework and a con- words, using masked language modeling and a next
  - Table 3: Performance comparison between existing pre- Table 4: Performance comparison between TAPT and
  - 4.4 Performance comparisons followed the hyperparameters used in TAPT except
  - (Lewis et al., 2020), which is a RoBERTa-large DoKTra for a fair comparison. The possible maxi-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：10 times, 1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - proaches have been applied to boost their per- improved architectures or training methods con-
  - Additional pre-training with in-domain texts However, these models must be further improved
  - performance and even outperform the teach- Specifically, we focus on the applicability of knowl-
  - knowledge to the student, which is more efficient (2019) proposed ALBERT, which outperformed
  - We apply our framework to the biomedical do- (2019) observed that BERT is significantly under-
  - The main goal of the DoKTra framework is to pro- BioBERT model outperforms the BERT model in
  - and thus, have a positive effect on distillation. a confidence penalty improves both the calibration

## 6. Conclusion、局限与可复现性

- **结论段落线索**：matics, 32(3):432鈥440. In this study, we proposed the DoKTra framework as a domain knowledge transfer method for PLMs. 脌lex Bravo, Janet Pi帽ero, N煤ria Queralt-Rosinach, The experimental results from the biomedical, clini- Michael Rautschka, and Laura I Furlong. 2015. Ex- traction of relations between genes and diseases from cal, and financial domain downstream tasks demon- text and large-scale data analysis: implications for strated that our proposed framework could trans- translational research. BMC bioinformatics, 16(1):1鈥 fer domain-specific knowledge into a PLM, while 17. 1666 Jang Hyun Cho and Bharath Hariharan. 2019. On the Geoffrey Hinton, Oriol Vinyals, and Jeff Dean. 2015. efficacy of knowledge distillation. In Proceedings Distilling the knowledge in a neural network. arXiv of the IEEE/CVF International Conference on Com- preprint arXiv:1503.02531. puter Vision, pages 4794鈥4802. Jangho Kim, SeongUk Park, and Nojun Kwak. 2018. Dongha Choi and Hyunju Lee. 2020. Extracting Paraph
- **局限/未来工作线索**：tillation, which focuses on the activation of tional pre-training has several limitations, such as；can be fairly compared in terms of performance is left as a future work.；In this study, we selected the FinBERT (Yang teacher model. However, the limitations of our
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Domain Knowledge Transferring for Pre-trained Language Model via Calibrated Activation Boundary Distillation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
