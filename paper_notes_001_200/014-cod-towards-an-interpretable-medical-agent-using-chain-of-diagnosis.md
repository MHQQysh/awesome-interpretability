# 014. CoD, Towards an Interpretable Medical Agent using Chain of Diagnosis

> 逐篇阅读记录：第 14 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Junying Chen, Chi Gui, Anningzhe Gao, Ke Ji, Xidong Wang, Xiang Wan, Benyou Wang
- **发表 venue / date**：ACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-acl.740)
- **领域标签**：Evaluation, Reasoning, LLM, Behavior, Control
- **本地 PDF 文本规模**：约 14,354 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：The field of AI healthcare has undergone a significant transformation with the advent of large language models (LLMs), yet the challenge of interpretability in these models remain largely unaddressed.This study introduce
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction box decision-making process into a diagnostic；1) We introduce the Chain-of-Diagnosis (CoD)；2 Preliminaries should be more transparent, ensuring it does；1. Symptom Abstraction；2. Candidate Disease Recall Explicit Symptoms ( ):；4. Confidence Assessment Diagnostic Reasoning and；5. Decision Making；5 Conclusion
- **引言关键线索**：chain that mirrors a physician鈥檚 thinking process In AI healthcare, automatic diagnosis (Tang et al., through five steps. For decision transparency, CoD 2016; Xu et al., 2019a; Fansi Tchango et al., 2022), outputs a confidence distribution, where higher con- which aims to provide convenient medical care and fidence indicates a stronger belief in diagnosing a assist in diagnosis, is one of the most promising ap- specific disease. This allows for control over the plications and is garnering increasing attention (Liu LLM鈥檚 decisions using a confidence threshold. Ad- et al., 2022; Hou et al., 2023; Hu et al., 2024; Yuan ditionally, diagnostic uncertainty can be quantified and Yu, 2024). However, it is complex, challenging by the entropy of these confidence levels. The goal the agent with multi-step decision-making abilities of entropy reduction can aid in eliciting more effec- (Chen et al., 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：remain largely unaddressed. This study offer a promising path due to their superior rea- introduces Chain-of-Diagnosis (CoD) to soning and dialogue abilities (Barua, 2024). These enhance the interpretability of medical capabilities enable them to address a wide range automatic diagnosis. CoD transforms the of diseases and interact effectively with patients diagnostic process into a diagnostic chain (Chen et al., 2023a). that mirrors a physician鈥檚 thought process, providing a transparent reasoning pathway. In this paper, we explore the use of LLMs for Additionally, CoD outputs the disease confi- automatic diagnosis. In our preliminary experi- dence distribution to ensure transparency in ments, we find that LLMs, like GPT-4, tend to decision-making. This interpretability makes make arbitrary diagnoses without sufficient inquiry. model diagnostics controllable and aids in Without interpretability, it is unclear if the decisions identifying
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, LLM, Behavior, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：This Traditional baselines, Non-LLM, Traditional, We compared four models, Basic Dataset, Disease, Symptom, Test Data, LLM baselines Our comparison, Table 2, Comparison of DxBench with, Comparison Results Table 3, Chain-of-Diagnosis in two as-, DiagnosisGPT, LLMs, Using CoD, Additionally, Comparison with Large Reasoning
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - mation from the disease encyclopedia data. This Traditional baselines (Non-LLM) Traditional
  - evaluations. We compared four models: Basic Dataset # Disease # Symptom # Test Data
  - LLM baselines Our comparison mainly focused Table 2: Comparison of DxBench with other datasets.
  - Comparison Results Table 3 presents the results
  - et al., 2022) and Chain-of-Diagnosis in two as- DiagnosisGPT_baseline 55.2 0.0 58.4 0.0
  - nary prompts. decision, akin to other LLMs. DiagnosisGPT_baseline

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significant transformation with the advent
  - dence distribution to ensure transparency in ments, we find that LLMs, like GPT-4, tend to
  - that DiagnosisGPT outperforms other LLMs tom inquiry capabilities, consistent with findings
  - agnosisGPT outperforms other LLMs with control- w/o inquiry w/ inquiry
  - decision-making and diagnostic abilities. Unlike not improve significantly and even decreases
  - inquire about symptoms to improve diagnostic ac- aspects inspire the design of the CoD framework,
  - with more inquiries, accuracy improves due to D鈥 = f2 (D, S, k) (2)

## 6. Conclusion、局限与可复现性

- **结论段落线索**：We conduct two ablation experiments with CoD training data: (1) w/o Confidence for Decision, In this paper, we propose the Chain of Diagnosis which learns to directly generate decisions like (CoD) to enhance the interpretability of LLMs for other LLMs, and (2) DiagnosisGPT_baseline, disease automatic diagnosis. Using CoD, we devel- 14352 oped DiagnosisGPT, an LLM capable of diagnos- Shenzhen Science and Technology Program ing of 9,604 diseases for validating CoD. Unlike (JCYJ20220818103001002), Shenzhen Doctoral other LLMs, DiagnosisGPT provides diagnostic Startup Funding (RCBS20221008093330065), confidence and performs open-ended reasoning us- Tianyuan Fund for Mathematics of National ing its own disease database. Experiments show Natural Science Foundation of China (NSFC) that the diagnostic capabilities of DiagnosisGPT (12326608), Shenzhen Science and Technology surpass those of other LLMs. Furthermore, higher Program (Shenzhen Key Laboratory Grant No. accuracy can be achieved by ad
- **局限/未来工作线索**：controllability in diagnostic rigor. Code, In response to these limitations, we propose the；tains real doctor-patient dialogues, we filtered out rounds, with a limitation of L = 5.；Limitations 2023. Sharegpt. https://sharegpt.com/. Accessed:；tasks, DiagnosisGPT has several limitations that anthropic. 2024. Introducing the next generation of
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“CoD, Towards an Interpretable Medical Agent using Chain of Diagnosis”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
