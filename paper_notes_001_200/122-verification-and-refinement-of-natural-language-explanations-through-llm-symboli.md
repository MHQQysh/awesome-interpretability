# 122. Verification and Refinement of Natural Language Explanations through LLM-Symbolic Theorem Proving

> 逐篇阅读记录：第 122 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xin Quan, Marco Valentino, Louise A. Dennis, André Freitas
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.172)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 14,018 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Natural language explanations represent a proxy for evaluating explanation-based and multi-step Natural Language Inference (NLI) models.However, assessing the validity of explanations for NLI is challenging as it typical
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Explanation-Refiner；2. We integrate Neo-Davidsonian event seman-；1. To infer the hypothesis, we need to find the；80 Number of Iterations 8 80 Number of Iterations 8 80 Number of Iterations 8；60 Llama2-70b 60 Llama2-70b 60 Llama2-70b；8 Refined e-SNLI 8 Refined QASC 8 Refined WorldTree；7 Mistral-small 7 Mistral-small 7 Mistral-small；6 GPT-4 6 GPT-4 6 GPT-4
- **引言关键线索**：2023a; Olausson et al., 2023; Jiang et al., 2024b) A recent line of research in Natural Language Infer- and enhance the quality of crowd-sourced explana- ence (NLI) focuses on developing models capable tions. In particular, we present a neuro-symbolic of generating natural language explanations in sup- framework, named Explanation-Refiner, that in- port of their predictions (Thayaparan et al., 2021; tegrates a Theorem Prover (TP) with Large Lan- Chen et al., 2021; Valentino et al., 2022a; Bostrom guage Models (LLMs) to investigate the following et al., 2022; Weir et al., 2023). Since natural lan- research questions: RQ1: 鈥淐an the integration of guage explanations can be used as a proxy to evalu- LLMs and TPs provide a mechanism for automatic ate the underlying reasoning process of NLI models verification and refinement of natural language ex- (Kumar and Talukdar, 2020; Zhao and Vydiswara
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：ity (Camburu et al., 2020; Wiegreffe and Maraso- Natural language explanations represent a vic, 2021; Valentino et al., 2021; Atanasova et al., proxy for evaluating explanation-based and 2023; Quan et al., 2024; Dalal et al., 2024), includ- multi-step Natural Language Inference (NLI) ing the adoption of language generation metrics models. However, assessing the validity of ex- for a direct comparison between models鈥 generated planations for NLI is challenging as it typi- cally involves the crowd-sourcing of apposite explanations and human-annotated explanations. datasets, a process that is time-consuming and However, this process is subject to different prone to logical errors. To address existing lim- types of limitations. First, the use of language itations, this paper investigates the verification generation metrics requires the crowd-sourcing and refinement of natural language explana- of explanation corpora to augment existing NLI 
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Davidsonian-event semantics. Figure 14
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - for a direct comparison between models鈥 generated
  - 12 also shows the comparison of average processed Davidsonian-event semantics. Figure 14 displays

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：97.3
Percentage
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and TPs improve the logical validity of human-
  - significantly outperforming open-source models of the theory 螛 is subsequently determined by a
  - forms, such as FOL, poses significant challenges
  - addresses one or more logical errors, continually re- demonstrated a consistent improvement in refining
  - tional details on the prompts used for refinement feedback but also shows significant differences be-
  - 4.1 Datasets all models demonstrate a consistent improvement
  - tion: e-SNLI, QASC, and WorldTree, using a total tably, GPT-4 outperformed other models, improv-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Existing work has explored robust and effective ap- In this work, we present a novel neuro-symbolic proaches for multi-hop reasoning tasks in explana- framework, Explanation-Refiner, which integrates tion generation (Thayaparan et al., 2021; Valentino LLMs and theorem provers for automatic verifi- et al., 2022b; Neves Ribeiro et al., 2022). In prior cation and refinement of natural language expla- research, metrics such as Mean Average Precision nations through iterative cycles. Extensive exper- (MAP) (Valentino et al., 2022a) have been em- iments on textual entailment and multiple-choice ployed to assess the ranking of facts in explana- QA tasks showed improved logical validity of tion generation tasks against gold-standard explana- human-annotated explanations. We investigated tions. Although these metrics effectively measure the model鈥檚 performance from simple to complex precision relative to these standards, they inade- explanatory/sentence structures and introduced a quately captu
- **局限/未来工作线索**：prone to logical errors. To address existing lim- types of limitations. First, the use of language；sons, 1990) coupled with First-Order Logic (FOL) vealing the strengths and limitations of differ-；comings are particularly critical in tasks that re- In future work, we aspire to enhance the frame-；assessments of logical validity. Although some Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Verification and Refinement of Natural Language Explanations through LLM-Symbolic Theorem Proving”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
