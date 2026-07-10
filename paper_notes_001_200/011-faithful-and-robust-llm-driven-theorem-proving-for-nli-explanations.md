# 011. Faithful and Robust LLM-Driven Theorem Proving for NLI Explanations

> 逐篇阅读记录：第 11 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xin Quan, Marco Valentino, Louise A. Dennis, Andre Freitas
- **发表 venue / date**：ACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.acl-long.867)
- **领域标签**：Representation, Rationale, Hidden, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 13,027 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Natural language explanations play a fundamental role in Natural Language Inference (NLI) by revealing how premises logically entail hypotheses.Recent work has shown that the interaction of large language models (LLMs) w
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；4. We also perform a manual evaluation to assess apply a quantifier and a logical consistency check；2 Automated Theorem Proving for tool (Paulson and Blanchette, 2012) within Is-；3 Methodology ing theorem provers with LLMs, especially for；V NP-OBJ may still prove a theorem within a formal system,；4 Empirical Evaluation；60 Our-GPT-4o 60 Our-GPT-4o 60 Our-GPT-4o；70 Ours-Deepseek-V3 70 Ours-Deepseek-V3 70 Ours-Deepseek-V3
- **引言关键线索**：art LLM-based theorem proving framework for Recent studies in Natural Language Inference NLI, Explanation-Refiner (Quan et al., 2024b). In (NLI) have developed models to leverage natu- particular, we explore methodologies to improve ral language explanations as a mechanism for rea- the faithfulness of autoformalisation and deliver soning in support of a hypothesis (Wiegreffe and a more robust way to effectively and efficiently 1 Code and data are available at: https://github.com/neuro- provide logically valid explanations. We further symbolic ai/faithful_and_robust_nli_refinement examine how varying degrees of dataset complex- 17734 Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 17734鈥17755 July 27 - August 1, 2025 漏2025 Association for Computational Linguistics shows "鈭儀 y. Person x 鈭 Alone x 鈭 Field y 鈭 鈭儀. OnePers
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tions. However, TPs require translating natural language into machine-verifiable formal repre- ential and linguistic capabilities of large language sentations, a process that introduces the risk of models (LLMs) by integrating them with external semantic information loss and unfaithful inter- theorem provers (TPs) to automatically verify the pretation, an issue compounded by LLMs鈥 chal- logical validity of explanations for NLI (Pan et al., lenges in capturing critical logical structures 2023; Olausson et al., 2023; Quan et al., 2024b; with sufficient precision. Moreover, LLMs are Dalal et al., 2024). still limited in their capacity for rigorous and However, these integrated neuro-symbolic ap- robust proof construction within formal verifi- cation frameworks. To mitigate issues related proaches still face notable challenges. First, auto- to faithfulness and robustness, this paper in- mated theorem provers (ATP) require a machine- vestiga
- **方法与解释性关系**：该论文主要围绕 `Representation, Rationale, Hidden, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 1, Comparison of our approach, Explanation-Refiner on different LLMs, Init, We compare our entific, Comparison of manually evaluated, Refiner, Quan et al, Bottom
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Table 1: Comparison of our approach with Explanation-Refiner on different LLMs across three datasets. Init.
  - each comprising 100 instances. We compare our entific explanations requiring multi-hop reasoning.
  - Comparison of manually evaluated variable, implication, and quantifier errors in the autoformalisation process from
  - Refiner (Quan et al., 2024b) as our baseline work,
  - refined across three datasets. Bottom 鈥 Comparison of manually evaluated variable, implication, and quantifier errors

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - verify and improve the validity of NLI explana-
  - yield significant improvements in autoformali- rial inferences (Pan et al., 2023; Olausson et al.,
  - LLM-TP architecture can substantially improve nations) into cohesive proofs and effectively self-
  - (NLI) have developed models to leverage natu- particular, we explore methodologies to improve
  - ity in multi-hop reasoning affect the reliability of posed framework improves the faithfulness of aut-
  - use LLMs to refine these errors explicitly from the improvement of 29.5%, 51.5%, and 41.25%
  - WorldTree (Jansen et al., 2018) shows that the pro- work significantly improve the faithfulness of

## 6. Conclusion、局限与可复现性

- **结论段落线索**：with "False". We then attempt to prove this modi- abelle/HOL, proof tactics are commands that sys- fied theorem, if the TP finds a proof, it indicates a tematically decompose complex proofs into sim- contradiction within the axioms. In this case, we pler sub-goals, automating routine steps such as use an LLM to refine the axioms and attempt to simplification. Typically, one is asked to prove a solve the contradictions. statement X given assumptions Y by using proof tactics Z, where Z includes commands like simp 3.3 Proof, Verification and Refinement (for simplification), auto (for automatic reasoning), After autoformalisation checking and refinement, and blast (for first-order reasoning). These tac- we employ the theorem prover T P to verify the log- tics instruct Isabelle鈥檚 proof engine on how to pro- ical validity of the axioms and determine whether cess a proof step by applying appropriate rules, A /= 蟿 holds. We first use the Sledgehammer tool simplifications, or other reasoning me
- **局限/未来工作线索**：Limitations Qianglong Chen, Feng Ji, Xiangji Zeng, Feng-Lin；Future work may explore more advanced semantic Preprint, arXiv:2412.19437.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Faithful and Robust LLM-Driven Theorem Proving for NLI Explanations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
