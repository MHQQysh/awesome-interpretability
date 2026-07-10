# 037. Are self-explanations from Large Language Models faithful?

> 逐篇阅读记录：第 37 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Andreas Nygaard Madsen, Sarath Chandar, Siva Reddy
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.19)
- **领域标签**：Attribution, Rationale, Reasoning, Hallucination, Behavior, Explain
- **本地 PDF 文本规模**：约 28,274 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Instruction-tuned Large Language Models (LLMs) excel at many tasks and will even explain their reasoning, so-called selfexplanations.However, convincing and wrong self-explanations can lead to unsupported confidence in L
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Interpretability-faithfulness of；5 Results；100. Bhosale, Dan Bikel, Lukas Blecher, Cristian Canton；2. Worst Worst；2. Office；1. Back；1) Yes 1) Yes；1) Yes announced it was boosting the allowance to 250MB to follow similar
- **引言关键线索**：Instruction-tuned large language models (LLMs), 鈥淣o鈥, assuming the self-explanation is faithful. Because we asked for an edit to get a 鈥淵es鈥 response, and the re- such as Llama2 (Touvron et al., 2023), Falcon sponse is 鈥淵es鈥, the counterfactual is faithful. Note the (Penedo et al., 2023), Mistral (Jiang et al., 2023), or self-explanation generation and self-consistency check GPT4 (OpenAI, 2023), are increasingly becoming must happen in two separate sessions. mainstream among the general population, due to their capabilities and availability. Instruction-tuned LLMs can even provide very their predictions but also on their self-explanations. convincing explanations for their utterances and However, it鈥檚 also well established that LLMs hal- will often do so unprompted. Because LLMs pro- lucinate (Bang et al., 2023; Yao et al., 2023). This duce these explanations themselves and they pro- cre
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：faithfulness, they have not previously been suc- cessfully applied to LLM self-explanations for Is the following candidate a good fit for a counterfactual, feature attribution, and redac- Senior SWE position? Answer only yes/no. tion explanations. Our results demonstrate that {insert counterfactual resume} faithfulness is explanation, model, and task- dependent, showing self-explanations should Yes not be trusted in general. For example, with sen- timent classification, counterfactuals are more Figure 1: Example of an LLM providing a counterfac- faithful for Llama2, feature attribution for Mis- tual self-explanation and using a self-consistency check tral, and redaction for Falcon 40B. to evaluate if it is faithful. 鈥 In this conversation with Llama2 (70B), we learn from the counterfactual edit that
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Reasoning, Hallucination, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - explanations, we suggest that self-explanation faith- challenging comparison
  - free-formed explanations, which cannot be as eas- this does mean that comparisons will be more chal-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：4x, 1x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - tion explanations. Our results demonstrate that {insert counterfactual resume}
  - fulness metrics that depend on confidence scores We find that the faithfulness of instruction-tuned
  - when critical information is missing. We achieve task dependence. Additionally, we show our find-
  - pattern significantly problematic.
  - In Section 4.2 and Section 4.3, we use a 鈥測ou鈥 persona may be significant if the model has
  - tion should not affect the classification and supports to improve the classification performance. Instead,
  - we find that only for RTE and bAbI-1 is Llama2- 100%

## 6. Conclusion、局限与可复现性

- **结论段落线索**：edits the CoT to contain false information (e.g. 鈥2 + 3 = 6鈥) and checks that the prediction follows. Our investigation reveals that self-explanations鈥 The issue here is that injecting false facts may cre- faithfulness is highly model and dataset-dependent. ate out-of-distribution results or be interpreted as This conclusion is similar to previous works (Lan- typos by the LLM, thus it鈥檚 unclear if this method ham et al., 2023; Madsen et al., 2022a; Bastings is completely valid. Regardless, they find similar et al., 2022). Our contribution is the ability to to our paper, that faithfulness is model and task- measure faithfulness on LLMs鈥 self-explanations, dependent. specifically counterfactuals, feature attribution, and 6.1 Non-faithfulness works redaction explanations. Self-consistency checks also have been used to ana- The task dependence is concerning as it means lyze other LLMs鈥 capabilities. For example, Kada- LLM self-explanations cannot generally be trusted. vath et al. (2022) an
- **局限/未来工作线索**：the explanation and classification generation. generally be trusted and propose how future work；7.1 Future work generates unfaithful explanations or inaccurate clas-；We propose that future work on developing sification, it鈥檚 a limitation of the model鈥檚 compre-；ily evaluated using self-consistency checks. lenging for future work. There may also be a class
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Are self-explanations from Large Language Models faithful?”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
