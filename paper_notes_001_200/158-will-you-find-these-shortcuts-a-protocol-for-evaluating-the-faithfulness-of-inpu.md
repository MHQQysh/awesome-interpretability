# 158. “Will You Find These Shortcuts?” A Protocol for Evaluating the Faithfulness of Input Salience Methods for Text Classification

> 逐篇阅读记录：第 158 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jasmijn Bastings, Sebastian Ebert, Polina Zablotskaia, Anders Sandholm, Katja Filippova
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.emnlp-main.64)
- **领域标签**：Attribution, Rationale, Evaluation, Transformer, Behavior, Analyze
- **本地 PDF 文本规模**：约 11,045 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Feature attribution a.k.a.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；3. Train two models of the same architecture class of problems that salience methods could be；4. Verify that the shortcut tokens can indeed be Single token (st): The simplest possible and still；6. Compute the faithfulness metrics by compar- Token in context (tic): Another realistic lexical；2. The model trained on the original data (the；3 Experimental Setup computed the minimum and mean accuracy on all；4 Results；7 Limitations
- **引言关键线索**：medical domain, for example, heatmaps over im- A prominent class of explainability techniques as- ages helped uncover so-called shortcuts (Geirhos sign salience scores to the input features, which et al., 2020) or spurious correlations between data reflect the importance of the features to the artifacts like doctor marks or tags and the predicted model鈥檚 decision. When applied to text classifiers disease1 (Codella et al., 2019; Sundararajan et al., those methods produce highlights over the input 2019; Winkler et al., 2019, inter alia). (sub)words. Interestingly, different methods may Spurious correlations plague NLP models too produce surprisingly dissimilar highlights. Figure 1 (Gururangan et al., 2018; Poliak et al., 2018; Be- shows this using the Language Interpretability Tool linkov et al., 2019; Rosenman et al., 2020; Geva (Tenney et al., 2020). So a natural question is: et al., 201
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Jasmijn Bastings Sebastian Ebert鈭 Polina Zablotskaia鈭 Anders Sandholm Katja Filippova Google Research {bastings,eberts,polinaz,sandholm,katjaf}@google.com Abstract Feature attribution a.k.a. input salience meth- ods which assign an importance score to a feature are abundant but may produce surpris- ingly different results for the same model on the same input. While differences are expected if disparate definitions of importance are as- sumed, most methods claim to provide faith- Figure 1: Salience maps produced by four common ful attributions and point at the features most methods on a sentiment classification example (SST2) relevant for a model鈥檚 prediction. Existing for a BERT model. The same token (eastwood) is as- work on faithfulness evaluation is not conclu- signed the highest (Grad-L2), the lowest (GxI, LIME) sive and does not provide a clear answer as to and a mid-range (IG) importance score (color intensity how different method
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Evaluation, Transformer, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Making such probabilities, Toxicity, Wulczyn et al, Random baseline, RAND, SST2 IMDB Toxicity or, UNK or, MASK, Li et al, LSTM models but does, BERT, Tab
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - rating is made explicit in the text. Making such probabilities, choice of baseline) may have a
  - treated differently by the model as compared with
  - 鈥 Toxicity (Wulczyn et al., 2017) is a varied ods and the Random baseline (RAND) to obtain
  - SST2 IMDB Toxicity or UNK or [ MASK ] vectors can serve as baseline
  - trained on a shortcut version of the training data. baseline and the original input x1:n in m steps. We
  - with the input embedding xi minus the baseline.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - lexical shortcuts apparent to the developer is thus a significant effect on its performance.
  - which would then indeed help them improve both of configurations of the four most popular
  - places the shortcut tokens on top of its salience ing suboptimal may outperform those claimed
  - models, GRADl2 achieves high precision and rank IG performance does not improve much with
  - datasets (Tab. 3 and 7). The lowest but still compar- significant improvement for LSTM models. Also
  - atively high precision (0.87) is on IMDB:tic where for BERT, the precision numbers improve only for
  - for IG when using BERT ( ). Using the [ MASK ] baseline (with logits) resulted in an improvement in the scores

## 6. Conclusion、局限与可复现性

- **结论段落线索**：et al., 2017) and there exist formal definitions of explanation fidelity (Yeh et al., 2019). However, We have argued for evaluating input salience meth- these have not been connected to model debugging ods with respect to how helpful they would be for where it is the top of a salience ranking that matters discovering shortcuts that are learned by the model. most. In the vision domain, our work is closest to This seems to be a clear use case from the model Adebayo et al. (2020, 2022), who also explore the developer perspective. To achieve this, we pro- debugging scenario with salience maps, Yang and posed a protocol for method evaluation and applied Kim (2019), who use synthetic data to obtain the it to three variants of lexical shortcuts (single to- ground truth for pixel importance, and Hooker et al. ken, token in context, and ordered pair) which are (2019), who contrast the performance of the same a proxy for shortcut heuristics that occur in com- model trained on original and modifi
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把““Will You Find These Shortcuts?” A Protocol for Evaluating the Faithfulness of Input Salience Methods for Text Classification”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
