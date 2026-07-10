# 130. Sources of Hallucination by Large Language Models on Inference Tasks

> 逐篇阅读记录：第 130 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Nick McKenna, Tianyi Li, Liang Cheng, Mohammad Javad Hosseini, Mark Johnson, Mark Steedman
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.182)
- **领域标签**：Probing, Evaluation, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 11,537 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Large Language Models (LLMs) are claimed to be capable of Natural Language Inference (NLI), necessary for applied tasks like question answering and summarization.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction itive hallucination: (1) LLM bias toward affirming；3 Experimental Design；I Entail was the；I Entail India exports tons of rice；I GenArg Entail location；4 Querying Models with Prompts；6 Experiment 2: when that entity is discussed in novel inferences:；8 Impact of Bias on Performance
- **引言关键线索**：Large Language Models (LLMs) such as LLaMA, entailment when the hypothesis may be attested in GPT-3/4, PaLM, and more (Touvron et al., 2023; the training text, including reliance on named entity Brown et al., 2020; Chowdhery et al., 2022), have identifiers; and (2) a corpus-frequency bias, affirm- been trusted by many to perform language un- ing entailment when the premise is less frequent derstanding in downstream tasks such as summa- than the hypothesis. rization, question answering, and fact verification, We establish that these biases originate from among others (Zhang et al., 2023). However, due the LLM pretraining objective, in which statisti- to the large-scale nature of LLM training on vast, cal modeling of the natural distribution of human- often proprietary data, and the inherent opacity generated text leads to (at the level of sentences) of LLM parameters, it is difficult to e
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：from previous studies. We demonstrate that LLM decision-making. We alter existing NLI LLMs perform significantly worse on NLI test datasets in targeted ways while measuring how pre- samples which do not conform to these biases dictions change, across several major LLM families than those which do, and we offer these as valu- (LLaMA, GPT-3.5, and PaLM). We demonstrate able controls for future LLM evaluation.1 two sources of LLM performance on the NLI task, which we offer as explanations of general false pos-
- **方法与解释性关系**：该论文主要围绕 `Probing, Evaluation, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Poliak et al, Levy, Holt consists of premise-hypothesis, FIGER types, Ling taining the same, Appendix A for, Entail and collapsing both, We compare LLMs, NLI, As discussed in Appendix, Jacob Devlin, Kenton Lee, Kristina N. Toutanova, Hypothesis only baselines in, We compared zero-shot and, IRandP rem, Entail predictions are false
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - hypothesis-only baseline (Poliak et al., 2018), but Levy/Holt consists of premise-hypothesis pairs,
  - each entity with one of the 48 FIGER types (Ling taining the same entailment label, as a baseline
  - widely-used comparison for their performance, and prepended before the query (see Appendix A for
  - enabling interesting comparisons. mal 4-example setup, which we find is capable of
  - pretrained, so it serves as a further comparison Entail and collapsing both B and C choices into
  - We compare LLMs鈥 performance between NLI

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：2.0 times, 1.9x, 2.2x, 2.0x, 1.6x, 1.8x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - LLMs perform significantly worse on NLI test datasets in targeted ways while measuring how pre-
  - esis is attested by the pretraining text. We find tion and generalization. Carlini et al. (2023) estab-
  - (3) For the NLI test samples consistent with not disprove generalization, but we show that GPT-
  - mance is severely degraded. We show that when
  - 2 Related Work we show the impact on real performance in 搂8.
  - we show that LLMs are inherently sensitive to attes- statement. We claim that when a statement is likely
  - of NLI datasets.2 data, it is more likely to affirm it as a conclusion in

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Additionally, Talman and Chatzikyriakidis NLI tasks, regardless of any premise it is presented (2019) report generalization failure among many with. We measure the attestedness of a sample models supervised for NLI 鈥 models fail to gen- by prompting the LLM to ask if the hypothesis in eralize between NLI datasets, even if the task is question is true, false, or unknown.3 Attestation formatted the same. On smaller Language Models predictions are denoted by 螞. such as RoBERTa (Liu et al., 2019; 355M params), A biased model will appear to perform well on Li et al. (2022) also observed a reliance on dataset dataset samples with entailment labels that happen to align with the bias. Table 1 shows examples 2 We speculate that a similar attestation effect could even from the Levy/Holt dev set. be present in the supervised models studied in Poliak et al. 3 (2018), which could contribute to those models鈥 performance. Alternatively, LLM perplexity for a statement could be We leave the investigati
- **局限/未来工作线索**：We leave the investigation of this to future work. used; however, this is not always available, e.g. with GPT-3.；sibly because the relative frequencies require gen- further exploration of this residual to future work.；further exploration should be done in future work. to a user鈥檚 query or contradiction of information
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Sources of Hallucination by Large Language Models on Inference Tasks”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
