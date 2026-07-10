# 061. Transformer-based Prediction of Emotional Reactions to Online Social Network Posts

> 逐篇阅读记录：第 61 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Irene Benedetto, Moreno La Quatra, Luca Cagliero, Luca Vassio, Martino Trevisan
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.wassa-1.31)
- **领域标签**：Rationale, Transformer, Behavior, Explain
- **本地 PDF 文本规模**：约 7,163 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Emotional reactions to Online Social Network posts have recently gained importance in the study of the online ecosystem.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction to provide end-users with explanations on the most；2 Related work external data from Google Trends and then apply-；6 Limitations；7 Ethical considerations a Shapley-based explainer to identify the tokens；8 Conclusions and Future Work Vishrav Chaudhary, Guillaume Wenzek, Francisco
- **引言关键线索**：Most Online Social Network (OSN) platforms al- influential textual features. low users to annotate posts with personal reactions. The main contributions of this paper can be sum- Reactions to social posts not only indicate the user marized as follows: sentiment (e.g., like, dislike) but also reflect emo- 鈥 The paper presents a new approach to pre- tions or feelings (e.g., sadness, love, care). More- dict emotional reactions to OSN posts ante- over, the quantity of reactions is also a direct mea- publication, i.e., disregarding post comments sure of the popularity of a post and, indirectly, can or replies. indicate the audience鈥檚 enthusiasm for a specific topic. Such annotations are particularly relevant to 鈥 It formulates the emotional reaction prediction marketers, advertisers, and policymakers because task as a multi-task regression problem, where they can be exploited to profile OSN u
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：publication, the number of received reactions tion prediction (Giachanou et al., 2018) rely on can be predicted based on either the textual con- tent of the post or the related metadata. How- traditional occurrence-based text statistics on text, ever, existing approaches suffer from both the e.g., TF-IDF (Manning et al., 2008). Thus, they lack of semantic-aware language understand- ignore the semantics behind the text. Furthermore, ing models and the limited explainability of the the prediction models are used as closed boxes and prediction models. To overcome these issues, do not provide any explanations of the predicted we present a new transformer-based method reaction. to predict the number of emotional reactions This paper proposes a new approach to emo- of different types to social posts. It leverages the attention mechanism to capture arbitrary tional reaction predictions based on Transform- semantic textual relations neglected b
- **方法与解释性关系**：该论文主要围绕 `Rationale, Transformer, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We compare Transformer+Metadata with, The prediction model exclusively, Table 2, Comparison between regression performance, MedAPE, The
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - We compare Transformer+Metadata with the
  - 鈥 The prediction model exclusively considers following baseline methods:
  - Table 2: Comparison between regression performance on test data for each reaction type, in terms of MedAPE. The

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - sition we find Comment and Share, respectively,
  - In this section, we show and discuss the results of
  - tively limited. Indeed, the improvements of Trans-
  - In this section, we show and discuss the perfor-
  - offers a statistically significant improvement with
  - respect to the given cell. The results show that
  - Trasformer-Based models improve significantly

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Guzma虂n, Edouard Grave, Myle Ott, Luke Zettle- The paper explored the use of transformer-based moyer, and Veselin Stoyanov. 2019. Unsupervised models to predict the number of reactions ante- cross-lingual representation learning at scale. CoRR, publication to a social post. It focused on predicting abs/1911.02116. the reaction counts for different types of positive Emilio Ferrara, Roberto Interdonato, and Andrea and negative reactions by exploring the role of text Tagarelli. 2014. Online Popularity and Topical Inter- and metadata information. Compared to prior ap- ests through the Lens of Instagram. In Proceedings 361 of the 25th ACM Conference on Hypertext and Social Luca Vassio, Michele Garetto, Emilio Leonardi, and Media, page 24鈥34. Carla Fabiana Chiasserini. 2022. Mining and mod- elling temporal dynamics of followers鈥 engagement Mehmetcan Gayberi and Sule Gunduz Oguducu. 2019. on online social networks. Social Network Analysis Popularity Prediction of Posts in Social Networks and 
- **局限/未来工作线索**：specifically, the Shapley values are computed using limitations and expand the scope of our findings.；the types of reactions a post receives. This infor- tions. Future work could explore the inclusion of；ology, alongside recognition of the limitations in；8 Conclusions and Future Work Vishrav Chaudhary, Guillaume Wenzek, Francisco
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Transformer-based Prediction of Emotional Reactions to Online Social Network Posts”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
