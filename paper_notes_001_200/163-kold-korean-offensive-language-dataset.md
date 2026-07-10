# 163. KOLD: Korean Offensive Language Dataset

> 逐篇阅读记录：第 163 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Younghoon Jeong, Juhyun Oh, Jongwon Lee, Jaimeen Ahn, Jihyung Moon, Sungjoon Park, Alice Oh
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.emnlp-main.744)
- **领域标签**：Evaluation, Transformer, Behavior, Detect
- **本地 PDF 文本规模**：约 9,504 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Recent directions for offensive language detection are hierarchical modeling, identifying the type and the target of offensive language, and interpretability with offensive span annotation and prediction.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Annotation Task Design tected characteristics, such as gender or reli-；4 Dataset Analysis；5 Experiments and Results Table 6: Evaluation results of offensive/target span pre-；8 Ethical Considerations laborating with such models, we can obtain large-；9 Limitations SelectStar provided a crowdsourcing platform for；2019. Fine-grained analysis of propaganda in news；1326. Kyumin Park, Jamin Shin, Seonghyun Kim, Lucy
- **引言关键线索**：2018; Karan and 艩najder, 2018), over-sensitivity to Online offensive language is a growing societal commonly-attacked identities (Dixon et al., 2018; problem. It propagates negative stereotypes about Kennedy et al., 2020), and propagating bias in an- the targeted social groups, causing representational notation (Sap et al., 2019; Davidson et al., 2019). harm (Barocas et al., 2017). Among various re- Among various attempts to solve those problems, search directions for offensive language detection, first is offensive language target detection to iden- 10818 Proceedings of the 2022 Conference on Empirical Methods in Natural Language Processing, pages 10818鈥10833 December 7-11, 2022 漏2022 Association for Computational Linguistics Label Level C Context (title) Comment Target Level A Level B Target Group Group Attribute 鞁頇旐晿電 鞝犽崝臧堧摫...SNS 霑岆? 靷霝岇潃 靷霝岇澕肟 NOT Gender in Conflict, deepening by SN
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：December 7-11, 2022 漏2022 Association for Computational Linguistics Label Level C Context (title) Comment Target Level A Level B Target Group Group Attribute 鞁頇旐晿電 鞝犽崝臧堧摫...SNS 霑岆? 靷霝岇潃 靷霝岇澕肟 NOT Gender in Conflict, deepening by SNS? People are all just people 頃滉淡 106臧 靷須岆嫧觳, 鈥橃晞頂勱皜雼堨姢韮 雮滊 氤错樃毂呪 靾橂 齑夑惮 鞝曥嫚膦 彀毽鞛 OFF UNT 鈥橮rotection of Afghan Refugees鈥 by 106 Korean civil societies Snap out of it 攵來暅, 鞎勴攧臧 靷韮滊 鞛愳嫚臧 鞏混棃雮... 氙戈淡鞐 鈥橃澑甓 氍胳牅鈥 鞐瓿 雸勱皜 旯鞝曥潃 鞚戈秾 膦 毵愳偞 鞎堩晿雮? OFF IND N. Korea calls US-led Afghan war 鈥橦uman Rights Crime鈥 Anyone for the extermination of Kim Jong-un ? 韹表啞 臧愳劚 瓯半秬頃橂嫟 氇豁暣 韺氩勲Μ電 頋戩澑 順曤嫎 氇鞚 韹表啞 鞚措澕電 鞚措勳潣 鞊半灅旮半姅 鞝滉卑霅橃暭霅 OFF OTH Angry TikTok reactions by Black bro The garbage named TikTok should be eliminated 靾橃垹氚涭潃 韸鸽灉鞀れ牋雿旊摛 鞚 氪愲弰 Gender & "鞚 氩曥潃 欤届潉 牖旐暅 雸勱惮毳 靷措Μ旮半弰 頃挫殧" 攵勲吀臧 靻熿晞 旃检潉 旰茧偞 欤届澊瓴犽嫟! OFF GRP Sexual LGBTQ+ "This law could save desperate lives who almost died" I get mad when I see transgender people who had Orient
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Transformer, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Zeinert et al, Zampieri et al, Table 4, Comparison of target group, Binary-F1 is used to, Table 7, Evaluation results of the, Level B, We compare the results, There is relatively little, In comparison, KOLD, Seaver
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - lease the dataset and baseline models. well (Zeinert et al., 2021; Zampieri et al., 2020).
  - Table 4: Comparison of target group distribution with els of each task. Binary-F1 is used to evaluate offensive
  - Table 7: Evaluation results of the multi-task baseline models compared against the single-task baseline models.
  - task model to predict the target type (Level B) and prediction model. We compare the results against our
  - the corresponding target span. baseline performance.
  - offensiveness and target classifications, we conduct we compare our baselines against (1) sequence clas-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - These improvements are focused on English Yes It is a mental disorder
  - span detection while having room for improve-
  - viding the context information improves the omy into a hierarchical annotation process to cre-
  - information of offensive language to improve of- 鈥 It is the first Korean dataset with a hierarchical
  - being a subclass of gender. We show the distribu-
  - becomes more significant as the granularity of the 5
  - English (61.6), the performance drops significantly in using automatically translated English datasets

## 6. Conclusion、局限与可复现性

- **结论段落线索**：els, Sap et al. (2020) collect social bias implicated about the targeted group in a free-text format. In a We present KOLD, a dataset of 40,429 comments similar spirit, Pavlopoulos et al. (2022) and Mathew of news articles and video clips, annotated within et al. (2021) create datasets annotated with particu- context. It is the first to introduce a hierarchical tax- lar span of the text that makes the post toxic (Zaidan onomy of offensive language in Korean with textual et al., 2007). As most text in the real world appears spans of the offensiveness and the targets. We es- in context (Seaver, 2015), considering context is tablish baseline performance for multi-task model important for the development of practical mod- that both detects the categories and the spans that els. Recent work on offensive language detection support the classification. Through analysis and ex- incorporates the context of the post (Vidgen et al., periments, we show that target terms are often omit- 2021; de Gib
- **局限/未来工作线索**：fer learning of English hate speech has limitations.；limitation, KOLD will serve as a stepping stone to Recently, there has been an approach to make a；an alternative to overcome such limitations. By col-；9 Limitations SelectStar provided a crowdsourcing platform for
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“KOLD: Korean Offensive Language Dataset”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
