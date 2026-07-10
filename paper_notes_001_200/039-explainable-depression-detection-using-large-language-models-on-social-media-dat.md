# 039. Explainable Depression Detection Using Large Language Models on Social Media Data

> 逐篇阅读记录：第 39 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yuxi Wang, Diana Inkpen, Prasadith Kirinde Gamaarachchige
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.clpsych-1.8)
- **领域标签**：Rationale, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 9,620 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Due to the rapid growth of user interaction on different social media platforms, publicly available social media data has increased substantially.The sheer amount of data and level of personal information being shared on
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3. Explored the use of LLMs for generating both；1 Introduction；4 Methodology enizer (PTB), and the outputs are 512-dimensional；1. The Top-5 Dataset IDEA-CCNL, SUS-Chat-34B3 is a bilingual；10. Crying；7 Conclusion and Future Work；1. Sadness；2. Pessimism
- **引言关键线索**：and Klein, 2005). Linguistic Inquiry and Word Being one of the leading global public health issues, Count (LIWC), a computerized text analysis tool, depression is common, costly, debilitating, and as- was developed to assess word usage in psycholog- sociated with an increased risk of suicide (Mar- ically meaningful categories (Tausczik and Pen- waha et al., 2023). Since depression has become nebaker, 2010). The tool was built by creating dic- a prevalent mental health issue, early detection of tionaries from domain knowledge, with the words symptoms could greatly improve the chances of categorized into different groups. proper treatment. Traditional methods of detection, In addition to social and semantic features, lin- usually human-led, are expensive to conduct and guistic n-gram features extracted from social media might be individually biased. In this study, we pro- data were used by
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Due to the rapid growth of user interaction on different social media platforms, publicly avail- that uses the selected writings to automatically fill able social media data has increased substan- in the Beck鈥檚 Depression Inventory (BDI) ques- tially. The sheer amount of data and level of tionnaire (Beck et al., 1961) for the social media personal information being shared on such plat- user (see Figure A1 for the full questionnaire). The forms has made analyzing textual information questionnaire then provides the level of depression to predict mental disorders such as depression a of the user based on all the answers. reliable preliminary step when it comes to psy- The main contributions of this paper are: chometrics. In this study, we first proposed a system to search for texts that are related to de- 1. Extended the applicability of using Large pression symptoms from the Beck鈥檚 Depression Language Models (LLMs) to predict mental Inven
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The comparison be-, Comparison to Related Work, Through comparisons
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - number of training epochs performed that was set records for several metrics. The comparison be-
  - 6.2 Comparison to Related Work Through comparisons, we can see that our sys-
  - forward compared with the features-based model

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1
         Percentage
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - step. Then, in this second step, we address 2. Improved the performance on the task of au-
  - symptoms could greatly improve the chances of categorized into different groups.
  - nary targets: at risk or non-risk of depression. Hus- task, with improved performance and with added
  - the top-5 relevant writings for each symptom nificant improvements on many benchmarks
  - on ADODL, which is an improvement considering the model also mentioned that no significant weight
  - Table 4: Our results compared to the state-of-the-art
  - enced significant changes in appetite, as they which can affect appetite. They also mentioned

## 6. Conclusion、局限与可复现性

- **结论段落线索**：task in eRisk 2019, 2020,and 2021 (Losada et al., The experimental results of our systems using 2019). The four metrics used for evaluation are: LLMs are shown in Table 3. 鈥 Average Hit Rate (AHR) We can learn that the usage of USESim for user The AHR is the hit rate averaged over all the writing selection is helpful, and it is generally bet- users. The hit rate measures the number of ter to have more writings kept so that the model answers systems automatically fill in that are could have more information about the user, and exactly the same as the actual answers pro- the writings would be more focused on the specific vided by the users. question. In our experiments, the usage of top-5 dataset leads to a better performance than using the 鈥 Average Closeness Rate (ACR) top-1 dataset. The ACR is the Closeness Rate averaged over When using the top-5 dataset, the model neural- all the users. It takes into account that the chat-7b-v3-1 performed better than SUS-Chat- answers represent an o
- **局限/未来工作线索**：knowledge about various depression-related symp- trustability. Due to limitations on the amount of；explanations. More importantly, no examples with Limitations；7 Conclusion and Future Work
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Explainable Depression Detection Using Large Language Models on Social Media Data”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
