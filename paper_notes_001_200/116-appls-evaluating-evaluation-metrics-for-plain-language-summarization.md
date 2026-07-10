# 116. APPLS: Evaluating Evaluation Metrics for Plain Language Summarization

> 逐篇阅读记录：第 116 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yue Guo, Tal August, Gondy Leroy, Trevor Cohen, Lucy Lu Wang
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.519)
- **领域标签**：Rationale, Evaluation, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 10,628 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：While there has been significant development of models for Plain Language Summarization (PLS), evaluation remains a challenge.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Work uation metric should be sensitive to based on both；5 Evaluation Metrics；2023. Chatgpt as a factual inconsistency evaluator；2023. Longeval: Guidelines for human evaluation Computational Linguistics (Volume 1: Long Papers),；2022. Template and guidance for writing a cochrane Annual Meeting of the Association for Computational；25 Generation；5 By presenting the reported score differences
- **引言关键线索**：Our goal is to assess how well existing metrics Plain language summaries of scientific informa- capture the multiple criteria of PLS. We identify tion are important to make science more accessible four criteria, informed by prior work (Pitcher et al., (Kuehne and Olden, 2015; Stoll et al., 2022) and 2022; Ondov et al., 2022; Stoll et al., 2022; Jain inform public decision-making (Holmes-Rovner et al., 2022), that a PLS metric should be sensitive et al., 2005; Pattisapu et al., 2020). Recently, gen- to: informativeness, simplification, coherence, and erative models have made gains in translating sci- faithfulness. We introduce a set of perturbations entific information into plain language approach- to probe metric sensitivity to these criteria, where able to lay audiences (August et al., 2023; Gold- each perturbation is designed to affect a single cri- sack et al., 2023; Devaraj et al., 2
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：captures all four criteria simultaneously. We Ondov et al., 2022) due to the multifaceted nature therefore recommend a suite of automated met- of the PLS task. Removal of unnecessary details rics be used to capture PLS quality along all (Pitcher et al., 2022), adding relevant background relevant criteria. This work contributes the first explanations (Guo et al., 2021), jargon interpreta- meta-evaluation testbed for PLS and a compre- tion (Pitcher et al., 2022), and text simplification fi hensive evaluation of existing metrics.1 (Devaraj et al., 2021) are all involved in PLS, pos- ing challenges for comprehensive evaluation.
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The resulting candidate summary, Figure 8, Comparison of BLEU scores, ACL, We exclude scores tween
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - and costly, making them only viable for the most robust baseline for evaluating the performance of
  - tion. The resulting candidate summary forms the Figure 8: Comparison of BLEU scores between oracle
  - ments over baseline (absolute value) reported in ACL鈥22
  - baseline model in the main text. We exclude scores tween the source, generated, and target texts.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - While there has been significant development Literature Criteria-specific design
  - Luo et al., 2023). We find that established metrics
  - aligns factually with the source text. improve understanding of health-related content.
  - improvements in ACL鈥22 summarization and generation papers are ROUGE (+0.89), BLEU (+0.69), METEOR
  - results in App. D). Median reported improvements
  - et al., 2023), and Claude.7 In Figure 5, we show adjectives, and term specificity decrease. Although
  - improvement. For informativeness, ROUGE,

## 6. Conclusion、局限与可复现性

- **结论段落线索**：duce the first鈥攖o our knowledge鈥攎eta-evaluation testbed, APPLS, for evaluating PLS metrics. Recent advances point to the possibility of au- tomated PLS; however, the multifaceted nature In APPLS, we apply controlled text perturba- 9201 tions to existing PLS datasets based on several crite- advancements in automated PLS and PLS evalua- ria (informativeness, simplification, coherence, and tion, aiming to foster more impactful, accessible, faithfulness). Using APPLS, we find that while and equitable scientific communication. some metrics reasonably capture informativeness, faithfulness, and coherence, SARI is uniquely sen- Limitations sitive to simplification perturbations, but exhibits Our perturbations use synthetic data to simulate insensitivity to other perturbations. Similar chal- real-world textual phenomena seen in PLS. Al- lenges are observed for QAEval, as no single met- though our approach is informed by prior work ric was consistently sensitive to all perturbations and provides
- **局限/未来工作线索**：Limitations of Existing Metrics The primary abstractive summarization (Sai et al., 2022; Koto；to CELLS, helping to address its limitations dis- the ratio of remaining to original sentences.；faithfulness, and coherence, SARI is uniquely sen- Limitations；have limitations as identified in our results. Fur- riorate with synthetic perturbations in a way that
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“APPLS: Evaluating Evaluation Metrics for Plain Language Summarization”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
