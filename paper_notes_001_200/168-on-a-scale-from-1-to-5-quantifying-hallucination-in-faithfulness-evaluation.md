# 168. On A Scale From 1 to 5: Quantifying Hallucination in Faithfulness Evaluation

> 逐篇阅读记录：第 168 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jing Xu, Srinivas Billa, Danny Godbout
- **发表 venue / date**：NAACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-naacl.433)
- **领域标签**：Rationale, Evaluation, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 9,824 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Hallucination has been a popular topic in natural language generation (NLG).In real-world applications, unfaithful content can result in poor data quality or loss of trust from end users.Thus, it is crucial to fact-check
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1111 Expedia Group Wy W；3 System scriptions given the grounding information such as；5 Conclusion to make an unfair comparison using only a；6 Limitations；1. Due to resource limitations, we focused on；2. Due to security concerns, the proxy service；4743. Patrick Lewis, Ethan Perez, Aleksandra Piktus, Fabio；8942. Percy Liang, Rishi Bommasani, Tony Lee, Dimitris
- **引言关键线索**：The recent advancement of large language models productionize an LLM-driven experience. With the (LLM) has made it easier than ever to build NLP ap- aim of adding another layer of security to ensure plications. With LLM鈥檚 powerful natural language the accuracy of LLM generated content, we inves- generation (NLG) ability, one can easily leverage tigate an automated approach to effectively score prompt engineering to generate texts. However, degrees of faithfulness in guided text generation. pretrained transformer decoders are prone to hal- Figure 1 illustrates a simple example of guided lucinations. Previously, it was shown that the generation scenarios. Given guidelines (the prompt performance of LLM鈥檚 decoding strategies (e.g., template) and grounding data (source facts), an beam search) has a dependency on the type of LLM model can hallucinate and generate either in- the generation tas
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：of synthetic unfaithful data, as well as heuris- tics to quantify the percentage of hallucination. nation, including retrieval-augmented generation Our results on 4 travel-domain industry dataset (Lewis et al., 2020), chain-of-thoughts (Wei et al., show that GPT-4 (Achiam et al., 2023) can pro- 2022), tree-of-thoughts (Yao et al., 2024), self- vide accurate judgement and explanation of consistency (Wang et al., 2023b), self-reflection (Ji whether a source and a generation are factually consistent. Furthermore, we found that tuning et al., 2023b), controlled decoding (Mudgal et al., NLI models on synthetic data can improve per- 2023), instruction tuning (Liu et al., 2023a). Nev- formance. Lastly, we present insights on the ertheless, the current state-of-the-art does not guar- latency and cost of deploying such a system. antee that hallucination can be 100% prevented. To apply NLG in industry applications, oftentimes
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We compared popular LLMs, The comparison, Figure 3, Baseline intrinsic and extrinsic, BERTScore, Zhang et al, Conclusion to make an, VLLM, Baseline 2.24 2.64 1.25
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - We compared popular LLMs as well as widely data, hallucination can happen in intrinsic or extrinsic
  - gan et al., 2005), given a premise, a model should a comparison in terms of the percentage of hallu-
  - The comparison (Figure 3) involves scoring both
  - 3.4 Baseline intrinsic and extrinsic hypotheses with 0% to 100%
  - BERTScore (Zhang et al., 2019) as our baseline. els tested are as follows: for rubric-based scoring
  - 5 Conclusion to make an unfair comparison using only a

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Our results on 4 travel-domain industry dataset (Lewis et al., 2020), chain-of-thoughts (Wei et al.,
  - NLI models on synthetic data can improve per- 2023), instruction tuning (Liu et al., 2023a). Nev-
  - grading with explanation can improve the LLMs
  - we showed that LLMs can accurately distinguish
  - training on synthetic data can further improve de-
  - TrueTeacher which improved both LLM- and NLI-
  - improve the performance of the NLI model. Lastly,

## 6. Conclusion、局限与可复现性

- **结论段落线索**：3) knowledge congruence which evaluates whether extrinsic information is injected to the hypothesis; In this section, we discuss the faithfulness scoring and 4) style alignment which scans whether the on the reference and hypothesis pairs for two sets language and tone in the hypothesis match with of experiments. The score ranges from "1-highly those from the reference. unfaithful", "2-very faithful", "3-slightly unfaith- ful", and "4-very unfaithful" to "5-highly faithful". We defined the grading rubric to follow a discrete It should be noted that HHEM models return a con- range from 1 to 5, with 5 being "highly faithful" tinuous probabilistic value in the range [0, 1]. It that all information in the hypothesis can be veri- was later scaled to [1, 5] for ease of interpretation. fied in the reference and 1 being "highly unfaithful" that most or all of the content in the hypothesis Our first experiment aims to evaluate the model cannot be validated from the reference. Further- scoring q
- **局限/未来工作线索**：One limitation is that the HHEM model has a preliminary accuracy on JsonTG was only promis-；model with acceptable performance as a first-pass 3. Due to resource limitations, we were unable；ity. Our future work includes exploring the score team for the related work on the LLM rubrics; Mag-；1. Due to resource limitations, we focused on
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“On A Scale From 1 to 5: Quantifying Hallucination in Faithfulness Evaluation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
