# 179. How Interpretable are Reasoning Explanations from Prompting Large Language Models?

> 逐篇阅读记录：第 179 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yeo Wei Jie, Ranjan Satapathy, Rick Siow Mong Goh, Erik Cambria
- **发表 venue / date**：NAACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-naacl.138)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 7,770 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Prompt Engineering has garnered significant attention for enhancing the performance of large language models across a multitude of tasks.Techniques such as the Chain-of-Thought not only bolster task performance but also
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction Self-Entailment-Alignment CoT (SEA-CoT) oper-；2 Motivation；3 Prompt Techniques；3. Our work is instead focused on enhancing the；5 Interpretability Qualities；8 Conclusion；7 Related Works
- **引言关键线索**：In recent trends, Large Language Models (LLM) ates similarly to Self-Consistency, but additionally have shown impressive performance across a di- utilizes a form of consistency between the corre- verse array of tasks, primarily through extensive sponding reasoning steps and supported context. scaling of model size (Brown et al., 2020). Tech- This action is missing in Self-Consistency, as the niques such as instruct-tuning (Wei et al., 2021) ap- focus is only on the resultant output, potentially plied across diverse tasks have empowered LLMs leading to unfaithful reasoning which may not sup- to execute inference on previously unseen tasks. port the underlying answer. One attributing factor lies with the extensive efforts Moreover, we conduct an extensive investiga- put into innovating new ways of prompting the tion into the reasoning explanations by evaluating LLM to better exploit their 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Prompt Engineering has garnered significant at- tention for enhancing the performance of large currently touches on one of the important aspects language models across a multitude of tasks. of utilizing these models for decision-making: in- Techniques such as the Chain-of-Thought not terpretability. The assumption is that the reasoning only bolster task performance but also delin- chain preceding the answer illustrates the model鈥檚 eate a clear trajectory of reasoning steps, of- thought process, enabling the audience to under- fering a tangible form of explanation for the stand how the answer is derived. However, such audience. Prior works on interpretability as- claims though seemingly plausible should be taken sess the reasoning chains yielded by Chain-of- Thought solely along a singular axis, namely lightly as they may not be faithful to the model鈥檚 faithfulness. We present a comprehensive and reasoning process (Jacovi and Goldberg, 2
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：SEA-CoT surpasses all other, Although SC-CoT is, SC-CoT that randomly picks, Huang majority answer. The, Table 1 demon-
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - phrasing as an evaluation of robustness in the fol- baseline, Ms (fs (x)) from Ms (fs (e虃 鈯 x)), where
  - answer the question. SEA-CoT surpasses all other baseline methods
  - over majority of the baselines. Although SC-CoT is
  - "shunned" is mentioned while other baselines used
  - thus exhibiting a tenuous connection to the ques- plement a baseline of SC-CoT that randomly picks
  - compared to other baselines, aligning with (Huang majority answer. The results from Table 1 demon-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：4 x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Prompt Engineering has garnered significant at-
  - than 70% improvements across multiple di-
  - improvements across multiple arithmetics and forms of acquiring feedback, such as querying
  - problems significantly facilitates the problem-
  - two sets of key tokens1 , which we show in later with multiple desirable traits concerning various
  - robust is the explanation against minor variations, the student model, to measure utility from improve-
  - bling multiple real-world facts to successfully We show the full experimental results in Figure 5.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：未稳定识别 Conclusion 段落。
- **局限/未来工作线索**：tions against such beliefs, identifying limitations；limitation of narrowing the predictors鈥 context, gen- generate text serving diverse objectives. In partic-；via maximizing ST . One caveat is that in the event；9 Limitations Antonia Creswell and Murray Shanahan. 2022. Faith-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“How Interpretable are Reasoning Explanations from Prompting Large Language Models?”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
