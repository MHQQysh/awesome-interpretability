# 104. Unveiling Factual Recall Behaviors of Large Language Models through Knowledge Neurons

> 逐篇阅读记录：第 104 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yifei Wang, Yuheng Chen, Wanting Wen, Yu Sheng, Linjing Li, Daniel Dajun Zeng
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.420)
- **领域标签**：Analysis, Hallucination, Hidden, Behavior, Analyze
- **本地 PDF 文本规模**：约 9,910 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，通过表征分析、因果干预或解释评估来连接内部机制与外部行为。
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；4 Diagnose the Pitfalls of Factual Recall；9 Conclusions；2023. Selection-inference: Exploiting large language 2023 Conference on Empirical Methods in Natural；5. Which organization is behind the；1. Who is the chairperson of General；3. Who presides over General Motors as its；4. Who currently serves as the chairperson ing KNs for each fact triplet proves to be the most
- **引言关键线索**：reasoning heavily relies on the factual knowledge Recent advancements in Large Language Mod- encoded within LLMs, acquired through extensive els have underscored their exceptional reason- pretraining on vast corpora, rather than on user- ing prowess with natural language understanding inputted premises. At the same time, it differs across a broad spectrum of tasks (Chen et al., from commonsense reasoning (Zhao et al., 2023; 2023a; Kojima et al., 2022; Brown et al., 2020; Trinh and Le, 2019), which taps into general knowl- Creswell et al., 2023). However, amidst these edge acquired through dynamic training to foster achievements, a specific form of reasoning has a holistic understanding of the world, instead of been somewhat overlooked and insufficiently inves- emphasizing specific factual information. tigated: reasoning tasks that require the utilization Intuitively, it is reasonable to 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：November 12-16, 2024 漏2024 Association for Computational Linguistics crucial for several reasons. First, efficient use of integrates with the KN technique. We assess the parametric knowledge may significantly reduce re- level of factual recall at each reasoning step by in- liance on external data sources, thereby lowering troducing a novel metric, KN Scores. We examine operational costs of data retrieval and API usage. KN Scores under three conditions of two-hop rea- Second, this dynamic capability allows the knowl- soning: no CoT, zero-shot CoT, and few-shot CoT, edge within LLMs to flow and interconnect (Onoe unveiling the pitfalls existing in the reasoning pro- et al., 2023), showcasing these models as organic cess and the enhancement effect of CoT (Wei et al., entities rather than static information repositories 2022). Then we conduct targeted interventions on (Petroni et al., 2019). From a practical perspective, KNs to enhance or s
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hallucination, Hidden, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：SiLU, KNs are significantly, Scores, Using the KN Scores, CoT, CoT mitigates the forgetting, Random, To establish a baseline, LLaMA2-7B, KNs of both of, We compare these scores, Under few-shot CoT setting, The percentage of FF
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - in the middle and final layers. compared with the single-hop baselines.
  - 蠅il = SiLU(H(l) W1 )[i], 鈭蠅il 鈭 蠅 (4) hop queries as baselines since KNs are significantly
  - edge less frequently in comparison to the straight-
  - Scores. Using the KN Scores metric, we evaluate by a higher 鈭喯1 and 鈭喯2 compared with no CoT
  - zero-shot CoT mitigates the forgetting of factual in- afterward. (4) Random: To establish a baseline
  - formation to some extent for LLaMA2-7B, whereas for comparison with conditions (2) and (3), we

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - recall process directly improves reasoning per- chairperson, Marry Barra).
  - parametric knowledge may significantly reduce re- level of factual recall at each reasoning step by in-
  - pretraining (Vaswani et al., 2017). A significant explore how the presence of knowledge conflict
  - edge neurons is, the more significantly their corre- bridge entity first and then using it to solve the sec-
  - 蠅il = SiLU(H(l) W1 )[i], 鈭蠅il 鈭 蠅 (4) hop queries as baselines since KNs are significantly
  - tions of KNs for each-hop fact. Then we hook shot, markedly improves factual knowledge utiliza-
  - to significantly improve the recall of the second-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Models Mistral-7B LLaMA2-7B LLaMA3-8B ing two-hop facts (TT) benefits the model鈥檚 reason- MH SC MH SC MH SC ing performance. Specifically, with the presence of No CoT 55.75 44.25 60.51 39.49 71.89 28.11 CoT, the proportion of TT significantly increases Zero-shot 70.84 29.16 64.26 35.74 95.66 4.34 and the model鈥檚 reasoning accuracy improves sub- Few-shot 91.23 8.77 89.02 10.98 97.65 2.35 stantially. Table 4: The fraction of MH and SC in correctly an- 7 Impact of Contextual Conflict swered examples (TT+FT). MH denotes successful re- trieval of both facts while others denote by SC. The capacity of utilizing internal factual knowledge is contingent not solely upon the intrinsic properties 6.2 Results Analysis of LLMs, but is also significantly influenced by the According to Table 4, under normal conditions, a context within which they operate. This section considerable proportion of correctly answered ques- elucidates how the presence of knowledge conflicts tions under no CoT setting rely 
- **局限/未来工作线索**：Our work addresses these limitations by examining；Limitations William W. Cohen. 2023a. Program of thoughts；several limitations. ilmaz, and Antoine Bosselut. 2023b. Reckoning:；This approach not only surpasses the limitations im-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Unveiling Factual Recall Behaviors of Large Language Models through Knowledge Neurons”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
