# 079. What changed? Investigating Debiasing Methods using Causal Mediation Analysis

> 逐篇阅读记录：第 79 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Sullam Jeoung, Jana Diesner
- **发表 venue / date**：ACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.gebnlp-1.26)
- **领域标签**：Analysis, Hidden, LLM, Layer, Detect
- **本地 PDF 文本规模**：约 5,563 个词

## 1. Abstract 讲解

- **研究问题**：模型可能记忆敏感信息或表现出偏置，研究需要识别风险来源并评估干预是否会损伤效用。
- **摘要主线**：解决大模型具体计算路径、关键模块和因果机制难以拆解的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Previous work has examined how debiasing language models affect downstream tasks, specifically, how debiasing techniques influence task performance and whether debiased models also make impartial predictions in downstrea
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2019. Debiasing embeddings for reduced gen-；2019. Enhancing the measurement of social effects
- **引言关键线索**：Recent work has shown that pre-trained language In this study, we focus on gender bias as a type models encode social biases prevalent in the data of bias. We examine (1) stereotypical associations they are trained on (May et al., 2019; Nangia et al., between gender and professions in pre-trained lan- 2020; Nadeem et al., 2020). In response to that, guage models (SEAT) (May et al., 2019), (2) stereo- solutions to mitigate these biases have been de- types encoded in language models (CrowS-Pairs) veloped (Liang et al., 2020; Webster et al., 2020; (Nangia et al., 2020), and (3) differences in systems Ravfogel et al., 2020). Some recent papers also affecting users unequally based on gender (Wino- examined the impact of debiasing methods, e.g., Bias) (Zhao et al., 2018). These representational reduction of gender bias, on the performance of harms can impact people negatively because they down
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Sullam Jeoung Jana Diesner University of Illinois-Urbana Champaign {sjeoung2,jdiesner}@illinois.edu Abstract the models neurons, layers, and attention heads, and what kind of changes in language models are Previous work has examined how debiasing lan- introduced when debiasing methods are applied to guage models affect downstream tasks, specifi- cally, how debiasing techniques influence task downstream tasks. In this paper, we apply causal performance and whether debiased models also mediation analysis, which investigates the infor- make impartial predictions in downstream tasks mation flow in language models (Pearl, 2022; Vig or not. However, what we don鈥檛 understand et al., 2020), to scrutinize the internal mechanisms well yet is why debiasing methods have varying of mitigating gender debiasing methods and their impacts on downstream tasks and how debias- effects on toxicity analysis as a downstream task. ing techniques affect interna
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hidden, LLM, Layer, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：This suggests two, We consider these models, This indicates that the, Baseline CDA Dropout Jigsaw, SEAT 7b with debiasing, This indicates that their, GPT2 baseline Jacob Devlin, Ming-Wei Chang, Kenton Lee
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - compared to the baseline model. This suggests two
  - der bias measures compared to the baseline models.
  - We consider these models as baseline models. the debiasing effects, we calculated a different bias
  - the baseline models. This indicates that the the debiasing methods did what they are supposed
  - Baseline CDA Dropout Jigsaw
  - example, SEAT 7b with debiasing methods applied better than the baseline model, however, this im-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Our results suggest that debiasing methods show crease the prediction accuracy of more and less
  - the behaviors of attention heads. Our results Specifically, they fine-tuned the model on toxic-
  - making small, systematic improvements to input suring social biases in language models (Nangia
  - For example, the sentence 鈥楬er most significant \begin {aligned} \textrm {Total Effect} (\textrm {set-gender},\textrm {null};y)&=&&\\ \frac {y_\textrm {set-gender}(u)-y_\textrm {null}(u)}{y_{null}(u)}
  - dataset was rebalanced into 鈥楬is most significant
  - on the corpus which contains offensive and results show no decrease in effect size and no sig-
  - fine-tuned models showed a significant increase

## 6. Conclusion、局限与可复现性

- **结论段落线索**：resolution matters for entity extraction, relation ex- In this work, we have investigated how debiasing traction, and social network analysis. In 2009 IEEE methods impact language models, along with the Symposium on Computational Intelligence for Secu- downstream tasks. We found that (1) debiasing rity and Defense Applications, pages 1鈥8. IEEE. methods are robust after fine-tuning on downstream Lucas Dixon, John Li, Jeffrey Sorensen, Nithum Thain, tasks. In fact, after the fine-tuning, the debiasing and Lucy Vasserman. 2018. Measuring and mitigat- effects strengthened. However, this effect is not ing unintended bias in text classification. In Proceed- supported across another bias measure. This indi- ings of the 2018 AAAI/ACM Conference on AI, Ethics, cates the need for both debiasing techniques and and Society, pages 67鈥73. bias benchmarks to ensure generalizability. The Martin Hilbert, George Barnett, Joshua Blumenstock, causal mediation analysis suggests that (2) The neu- Noshir Con
- **局限/未来工作线索**：future work to focus on those components.；Several limitations apply to this work. We only
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“What changed? Investigating Debiasing Methods using Causal Mediation Analysis”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
