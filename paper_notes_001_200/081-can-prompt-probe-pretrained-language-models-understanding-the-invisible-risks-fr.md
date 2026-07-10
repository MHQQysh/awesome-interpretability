# 081. Can Prompt Probe Pretrained Language Models? Understanding the Invisible Risks from a Causal View

> 逐篇阅读记录：第 81 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Boxi Cao, Hongyu Lin, Xianpei Han, Fangchao Liu, Le Sun
- **发表 venue / date**：ACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.acl-long.398)
- **领域标签**：Probing, Evaluation, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 8,621 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Prompt-based probing has been widely used in evaluating the abilities of pretrained language models (PLMs).
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Structural Causal Model for Factual predictor I is first derived by combining PLM；10 BERT-large RoBERTa-large GPT2-xl BART-large；6 Sample Disparity Bias；9 Conclusions and Discussions；2019. Commonsense knowledge mining from pre-；2020 Conference on Empirical Methods in Natural
- **引言关键线索**：During the past few years, the great success of 2020a), BioLAMA (Sung et al., 2021), etc. pretrained language models (PLMs) (Devlin et al., Unfortunately, recent studies have found that 2019; Liu et al., 2019; Brown et al., 2020; Raffel evaluating PLMs via prompt-based probing could et al., 2020) raises extensive attention about eval- be inaccurate, inconsistent, and unreliable. For uating what knowledge do PLMs actually entail. example, Poerner et al. (2020) finds that the per- One of the most popular approaches is prompt- formance may be overestimated because many in- based probing (Petroni et al., 2019; Davison et al., stances can be easily predicted by only relying 2019; Brown et al., 2020; Schick and Sch眉tze, on surface form shortcuts. Elazar et al. (2021) 2020; Ettinger, 2020; Sun et al., 2021), which shows that semantically equivalent prompts may assesses whether PLMs are knowledg
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：and more reliable evaluations of pretrained lan- corresponding tasks. Such a probing evaluation guage models. Furthermore, our conclusions has been wildly used in many benchmarks such also echo that we need to rethink the criteria for as SuperGLUE (Wang et al., 2019; Brown et al., identifying better pretrained language models1 . 2020), LAMA (Petroni et al., 2019), oLMpics (Tal- mor et al., 2020), LM diagnostics (Ettinger, 2020),
- **方法与解释性关系**：该论文主要围绕 `Probing, Evaluation, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：To this end, PLM evaluation via tions, PLMs and verbalized probing, Figure 4 demonstrates an
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - To this end, we compared PLM evaluation via tions between PLMs and verbalized probing
  - and faithful but vary in linguistic expressions. ther lead to unstable comparisons between different
  - comparisons. Figure 4 demonstrates an exam-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：0
                                                                         X, 10 point
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - results and conclusions, and proposes to con- evaluation criteria.
  - guage models. Furthermore, our conclusions has been wildly used in many benchmarks such
  - based probing datasets, and take PLMs鈥 perfor- standing its inherent vulnerabilities, are significant.
  - wrong conclusions. Therefore, to reach a trustwor- a prompt to PLMs鈥 linguistic preference. For
  - task distributional correlations may significantly invisible risks, to understand how spurious correla-
  - As a result, such task distributional correlations language models. Consequently, our conclusions
  - in Figure 2a. Based on the SCM, we find that guage models with widely used prompt-based

## 6. Conclusion、局限与可复现性

- **结论段落线索**：duct debiasing via causal intervention. This pa- per provides valuable insights for the design of mance on these datasets as their abilities for the unbiased datasets, better probing frameworks and more reliable evaluations of pretrained lan- corresponding tasks. Such a probing evaluation guage models. Furthermore, our conclusions has been wildly used in many benchmarks such also echo that we need to rethink the criteria for as SuperGLUE (Wang et al., 2019; Brown et al., identifying better pretrained language models1 . 2020), LAMA (Petroni et al., 2019), oLMpics (Tal- mor et al., 2020), LM diagnostics (Ettinger, 2020),
- **局限/未来工作线索**：into our SCM, delegate this to future work.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Can Prompt Probe Pretrained Language Models? Understanding the Invisible Risks from a Causal View”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
