# 065. Evaluating Open-Domain Question Answering in the Era of Large Language Models

> 逐篇阅读记录：第 65 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ehsan Kamalloo, Nouha Dziri, Charles L. A. Clarke, Davood Rafiei
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.acl-long.307)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 10,483 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Lexical matching remains the de facto evaluation method for open-domain question answering (QA).
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related Work；6 Regex Matching on CuratedTREC；7 Conclusion International Conference of the cross-language eval-
- **引言关键线索**：ure 1, 鈥淛icheng鈥 is a correct answer to what was Reliable benchmarks have been a bedrock to mea- the city of Beijing previously known as? while not suring progress in open-domain QA, the task of an- annotated as a gold answer in Natural Questions- 1 Code and data are released at https://github.com/ 2 ehsk/OpenQA-eval. typically equipped with a search engine 5591 Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics Volume 1: Long Papers, pages 5591鈥5606 July 9-14, 2023 漏2023 Association for Computational Linguistics OPEN (NQ- OPEN ; Lee et al. 2019). which is why, InstructGPT (zero-shot) is overes- With the recent success of generative QA sys- timated under these models, compared to human tems in the open-domain setting (Izacard and judgment. Grave, 2021b; Roberts et al., 2020), it becomes We repeated this experiment with the 20-year- harder for lexical 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：swering (QA). Unfortunately, lexical matching fails completely when a plausible candidate an- Who won the Oscar for best actor in 1975? swer does not appear in the list of gold answers, Lexical Jack Nicholson won which is increasingly the case as we shift from Match the Oscar for Best Actor in 1975 for extractive to generative models. The recent suc- Art Carney his performance in QA Model cess of large language models (LLMs) for QA One Flew Over the Cuckoo's Nest. aggravates lexical matching failures since can- didate answers become longer, thereby making Figure 1: Examples of failures in open-domain QA eval- matching with the gold answers even more chal- uation. Top: Jicheng is a credible answer although not lenging. Without accurate evaluation, the true present in the list of gold answers. Existing automated progress in open-domain QA remains unknown. evaluation mechanisms fail to identify it as correct. Bot- In this paper, we conduct
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Dataset, We make a comparison, LCCmain2002, Pasca and, EMNLP, Online, Association for Smrz. 2021, R2-D2
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - and draw a comparison between their estimated
  - Dataset. We make a comparison across open-
  - as baselines: LCCmain2002 (88.1%; Pasca and
  - (EMNLP), pages 6521鈥6532, Online. Association for Smrz. 2021. R2-D2: A modular baseline for open-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：40
points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - formance of all models is significantly under- swering information-seeking questions over a mas-
  - While it might be assumed that improved per-
  - some correct answers are missed, we show this as-
  - performance to be 71.4%, a nearly +60% improve- are unlikely to be found in a knowledge base. More-
  - the absolute improvements are lower. However, because the presence of an input context often cur-
  - methods. Only under human assessment does InstructGPT (few shot) outperform all other models.
  - the two evaluation method. InstructGPT (few shot) outperforms other models only under human assessment.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Candidate: Jicheng Table 1 presents the accuracy of the open-domain Is candidate correct? QA models, computed using the three evaluation mechanisms, BEM, InstructGPT-eval, and Human, We include the gold answer along with the candi- compared to the de facto EM accuracy. The ac- date answer in the prompt, akin to the semantic curacy of all models consistently surges across all similarity mechanism, as the objective here is to verify the correctness of the candidate. We call this 5 The human annotators are the authors of this paper. 5594 Entire Data (3.6K) Sampled (301) BEM InstructGPT-eval Human Model K EM F1 EM F1 Acc 鈭 Acc 鈭 Acc 鈭 InstructGPT (zero-shot) - 14.6 - 12.6 27.5 63.5 +50.9 77.1 +64.5 71.4 +58.8 InstructGPT (few-shot) - 29.9 - 33.9 50.5 59.5 +25.6 67.8 +33.9 75.8 +41.9 DPR 50 40.9 47.8 45.9 52.3 52.5 +6.6 55.1 +9.2 58.8 +12.9 FiD 100 46.5 53.7 47.8 55.4 58.1 +10.3 61.5 +13.7 64.8 +17.0 ANCE+ & FiD 50 47.3 54.8 48.2 55.9 59.5 +11.3 63.1 +14.9 65.8 +17.6 RocketQAv2 & FiD 100 47
- **局限/未来工作线索**：Limitations Language models are few-shot learners. In Ad-；3 A1. Did you describe the limitations of your work?
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Evaluating Open-Domain Question Answering in the Era of Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
