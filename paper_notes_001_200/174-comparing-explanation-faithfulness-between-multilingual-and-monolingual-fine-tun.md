# 174. Comparing Explanation Faithfulness between Multilingual and Monolingual Fine-tuned Language Models

> 逐篇阅读记录：第 174 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Zhixue Zhao, Νικόλαος Αλέτρας
- **发表 venue / date**：NAACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.naacl-long.178)
- **领域标签**：Rationale, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 16,222 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Zhixue Zhao, Nikolaos Aletras.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；3 Experiments；4 Results；2020. Evaluating and characterizing human ratio- Jay DeYoung, Sarthak Jain, Nazneen Fatema Rajani,；I Full Results of Faithfulness
- **引言关键线索**：As shown in Figure 1, even for the same input Feature attribution methods (FAs) are commonly (鈥淧ersonne ne veut aller 脿 cette f锚te.鈥, i.e. 鈥淣o- used for ranking input tokens according to their body wants to go to this party.鈥 in English), model importance to a model鈥檚 prediction (Kindermans prediction and FA, the token importance scores et al., 2016; Sundararajan et al., 2017; DeYoung can be substantially different between multi- and et al., 2020). Subsequently, the top-k ranked tokens monolingual models. This indicates that the mod- are selected to form a rationale. The faithfulness of els follow different inner processes for making pre- a FA method refers to what extent its token impor- dictions. It is unclear whether this difference is tance scores and selected rationales actually reflect generally shared among input examples or even the model鈥檚 inner reasoning mechanism (Jacovi acros
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：vide insights into how different parts of the input contribute to a prediction. Previous stud- ies have explored how different factors affect Figure 1: Model explanations given by the same fea- faithfulness, mainly in the context of monolin- ture attribution method, e.g. attention, for multilingual gual English models. On the other hand, the (XLM-R) and monolingual (French RoBERTa) models differences in FA faithfulness between multi- for the same task (sentiment analysis in FR). lingual and monolingual models have yet to be explored. Our extensive experiments, cover- ing five languages and five popular FAs, show Aletras, 2022), adversarial attacks (Sinha et al., that FA faithfulness varies between multilin- 2021; Zhao et al., 2022a) and temporal shifts (Zhao gual and monolingual models. We find that the larger the multilingual model, the less faithful et al., 2022b) on the faithfulness of FAs. On the the FAs are compared to its counterp
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Sinha et al, Zhao et al, Performance comparison of monolingual, Serrano and Smith, We compare models in, We also include a, Soft Sufficiency, Comprehensiveness, Soft, Therefore, FAs, Pre-trained bert transformer models, Sameer Singh. 2021. An, Comparison of predictive performance
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Sinha et al. (2021) and Zhao et al. (2022a) explored 2.3 Performance comparison of monolingual
  - for a realistic comparison between models, given gradient (Serrano and Smith, 2019).
  - pre-training data). We compare models in various
  - baseline input (i.e. zero embedding vector) to
  - We also include a baseline that randomly assigns Soft Sufficiency & Comprehensiveness. Soft
  - tive likelihood between retaining only the rationale random baseline. Therefore, values greater than

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：4.7 times, 1.5 times, 2.2 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - gual and monolingual models. We find that the
  - 鈥 Our results reveal that the degree of faithful- lingual and bilingual corpora. Rama et al. (2020)
  - parity of FAs by looking at each one separately. els. IG is also the only FA showing significant dif-
  - significantly different between mono- and multi- Avg Diff 0.014 0.014 0.005 0.096 0.023 0.03
  - We find that the faithfulness disparity varies across of faithfulness disparities between RoBERTa- and
  - a 10% rationale length, with a significant average degree will increase.
  - from a different perspective. The results show that Second, RoBERTa-based models have larger

## 6. Conclusion、局限与可复现性

- **结论段落线索**：To the best of our knowledge, our study is the first Acknowledgements to investigate the faithfulness disparity between monolingual and multilingual models. We have ZZ and NA are supported by EPSRC grant conducted a comprehensive empirical study and EP/V055712/1, part of the European Commis- found that faithfulness gaps exist across languages, sion CHIST-ERA programme, call 2019 XAI: Ex- models, and FAs. Our study further reveals that plainable Machine Learning-based Artificial Intel- the larger the multilingual model, the less faith- ligence. We thank George Chrysostomou for his ful its rationales are compared to its monolingual invaluable feedback. 3234 References for model interpretability methods. In Proceedings of the 60th Annual Meeting of the Association for Wissam Antoun, Fady Baly, and Hazem Hajj. Computational Linguistics (Volume 1: Long Papers),
- **局限/未来工作线索**：models. We observe that rationales of multilingual Limitations；echoing the observations in Section 5.2. hope future work can contribute resources to facil-；faithfulness for future work.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Comparing Explanation Faithfulness between Multilingual and Monolingual Fine-tuned Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
