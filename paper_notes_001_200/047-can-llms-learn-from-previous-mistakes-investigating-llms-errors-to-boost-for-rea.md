# 047. Can LLMs Learn from Previous Mistakes? Investigating LLMs’ Errors to Boost for Reasoning

> 逐篇阅读记录：第 47 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yongqi Tong, Dawei Li, Sizhe Wang, Yujia Wang, Fei Teng, Jingbo Shang
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.169)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 9,186 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Large language models (LLMs) have demonstrated striking reasoning capability.Recent works have shown the benefits to LLMs from fine-tuning golden-standard Chain-of-Thought (CoT) rationales or using them as correct exampl
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction termediate errors occur in making CoT procedures；2 Related Work；5 Our Methodology: Mistake Tuning；9 Conclusions and Future Work Acknowledgements；2019. Mathqa: Towards interpretable math word
- **引言关键线索**：and whether LLMs can learn from those mistakes. Large language models (LLMs) (Brown et al., To address these issues, we aim to explore the po- 2020; Zhang et al., 2022; Anil et al., 2023; Tou- tential of LLMs to effectively utilize their previous 鈭 鈥燙orresponding author. mistakes to boost reasoning. 3065 Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 3065鈥3080 August 11-16, 2024 漏2024 Association for Computational Linguistics To enhance the scalability and efficiency of ana- of our approaches in leveraging LLMs鈥 mistakes lyzing and learning from the mistakes of LLMs, we during both the tuning and inference stages. Addi- began by collecting a vast dataset of LLMs鈥 reason- tionally, we conduct thorough analyses of the error ing outputs and built C OTE RROR S ET, which con- types exhibited by LLMs, offering comprehensiv
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：making such mistakes. To explore the effective- takes(Section 4 and Section 5). ness of those mistakes, we design two methods: (1) Self-rethinking prompting guides LLMs to rethink whether they have made similar previ- ous mistakes; and (2) Mistake tuning involves vron et al., 2023) have demonstrated strong capabil- finetuning models in both correct and incor- ities across various tasks and applications (Liang rect reasoning domains, rather than only tun- et al., 2022; Chang et al., 2023). To further un- ing models to learn ground truth in traditional leash the reasoning abilities of LLMs and align methodology. We conduct a series of experi- their thinking process with humans, many recent ments to prove LLMs can obtain benefits from studies explored Chain-of-Thought (CoT)-based mistakes in both directions. Our two meth- ods offer potentially cost-effective strategies prompting (Wei et al., 2022; Wang et al., 2022; Li by leveraging errors
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We conduct comparisons between, Baselines, We select the following, LogiQA dataset sourced from, Table 3, PaLM2, Self-rethinking, GPT4, OpenAI
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - We conduct comparisons between self-rethinking
  - and several other baselines on multiple bench-
  - Baselines: We select the following reason-
  - ing baselines to evaluate our framework, self- 鈥 LogiQA dataset sourced from expert-written
  - Table 3: PaLM2鈥檚 accuracy on the existing baselines and our methods, self-rethinking prompting. Self-rethinking
  - shows consistent improvements but uses less inference time compared with self-consistency.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：3 times, 0.2 x, 2 x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - pabilities, which costs significantly less than Besta et al., 2023) to instruct LLMs to solve the
  - our work. troduce self-improve that employs CoT plus self-
  - performance, we introduce C OTE RROR S ET, a in providing future improvements from a different
  - SNLI (Camburu et al., 2018) and ECQA (Aggar- various continents and oceans, can lead to significant
  - past errors. This method starts with an initial CoT ing rationales, mistake tuning can further improve
  - signed to assess if the model鈥檚 most recent response tivations and conclusions of self-rethinking, where
  - better ability to generate more satisfactory con- that enables LLMs to iteratively improve their

## 6. Conclusion、局限与可复现性

- **结论段落线索**：includes similar inaccuracies. If errors are detected, LLMs can learn from the implicit reasons and types it will formulate a new rationale and undergo the of mistakes they made in CoT reasoning. This pro- evaluation process again. This cycle continues un- cess can be formulated as: til the model deems its latest answer to be correct or it reaches a set limit of evaluation rounds. The p = [Q 鈯 S 鈯 R], (1) main goal is to empower the LLM to learn from its errors introspectively and minimize the recurrence /p/ of such mistakes. One example is shown in Table 2. X L=鈭 logP (pt /p<t ), (2) The core of self-rethinking lies in the backward- t=1 checking stage. In this phase, the LLM reviews its reasoning chain, but with a specific focus on the Where Q, S and R represent the given question, error types it previously identified. This explicit special prefix and corresponding rationale respec- demonstration of errors, coupled with the question, tively. 鈯 represents the operation of concatenation
- **局限/未来工作线索**：from GSM8K and LogiQA to conduct an in-depth limitations in comprehending the nuances of math-；ally arise due to the model鈥檚 limitations in under- cal errors in arithmetic reasoning.；9 Conclusions and Future Work Acknowledgements；For future work, we envision proposing corre- Annual Meeting of the Association for Computational
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Can LLMs Learn from Previous Mistakes? Investigating LLMs’ Errors to Boost for Reasoning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
