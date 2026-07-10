# 150. Automatic Prompt Augmentation and Selection with Chain-of-Thought from Labeled Data

> 逐篇阅读记录：第 150 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Kashun Shum, Shizhe Diao, Tong Zhang
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.811)
- **领域标签**：Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 16,336 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Chain-of-thought (CoT) advances the reasoning abilities of large language models (LLMs) and achieves superior performance in complex reasoning tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Motivation is observed that human-written CoT tends to be；5 Experimental Results nally, under the self-consistency setting, Automate-；6 Additional Experiments and Analysis CoT can consistently outperform random selection；7 Ablation Study；9 Conclusion Acknowledgments；10 Limitations nologies, Volume 1 (Long and Short Papers), pages；2020 Conference on Empirical Methods in Natural
- **引言关键线索**：All of these sensitivities make human-based prompt The recent success in large language models engineering costly and motivate us to find an au- (LLMs) has shown that properly prompted LLMs tomatic and task-agnostic way to adapt chain-of- demonstrate emergent capabilities on complex un- thought exemplars to any downstream tasks. derstanding and question-answering tasks (Wei In this paper, we solve these problems by a CoT et al., 2022a). Especially, with the recently pro- augmentation and selection process to find suitable posed chain-of-thought (CoT) prompting (Wei exemplars automatically. This can be divided into et al., 2022b), LLMs are capable of solving rea- three steps: (1) Augment: The language model soning tasks including arithmetic reasoning, com- generates multiple pseudo-chains for query ques- monsense reasoning, and symbolic reasoning. The tions automatically. (2) Prune: Based
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：competitive results are achieved on arithmetic the rationale chains; (3) diversity: the combina- reasoning (+2.7%), commonsense reasoning tion of different complex-level exemplars; (4) style (+3.4%), symbolic reasoning (+3.2%), and non- sensitivity (Papadopoulos et al., 2010): the writ- reasoning tasks (+2.5%).1 ing/linguistic style of the rationale chains. Detailed analysis of the four factors is covered in Section 2.
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Compared with Auto-CoT, Zhang et al, GSM8K, Then the baseline models, VR- 4.2 Baselines, PGE, Williams, Dong et al, Zhou We compare our, Diao et al, Manual-, Table 1, The overall performance of, Automate-CoT and the comparison, The experimental results are, Table 1. We baseline, Figure 3, Comparisons between Random Selection
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - effort. Compared with Auto-CoT (Zhang et al., hops (reasoning steps of rationale chains) on GSM8K.
  - (2022b). (3) sensitivity: the model performance metrics (搂 4.1). Then the baseline models (搂 4.2)
  - variance-reduced policy gradient estimator (VR- 4.2 Baselines
  - PGE) (Williams, 1992; Dong et al., 2020; Zhou We compare our method with the following
  - et al., 2021; Diao et al., 2022), a kind of reinforce- baseline methods: chain-of-thought (Manual-
  - Table 1: The overall performance of Automate-CoT and the comparison against existing models on eleven

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：10 times, 1 X, 1X, 40 times, 12x, 8x, 16 times, 15 times, 13 times, 20 times, 8 times, 3 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - ing abilities of large language models (LLMs) CoT, several recent studies improve it by leveraging
  - ity (Zhao et al., 2021; Zhang et al., 2022; Liu et al., complex exemplars can improve CoT performance.
  - we first investigate whether these sensitivities still that Complex-CoT can improve the accuracy of
  - exemplars. to improve the quality of the pool. Considering the
  - tent variables by AdamW (Loshchilov and Hutter, Automate-CoT improves Manual-CoT by 2.7%
  - the exemplars combination (arg max pi ) with the CoT improves SC by a large margin by an av-
  - on the test set. By default, we query the language CoT, Automate-CoT also outperforms it on all

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tively. The results show that our method can be domly select the exemplars from the pool, it is very generalized to various task types and is not limited likely to obtain a much lower accuracy than the to reasoning tasks. manually written method. However, our Automate-
- **局限/未来工作线索**：performance on several complex tasks like arith- ing results, there are still some limitations to our；parameters and gradients are not accessible, caus- Prompt Style Definition : Another limitation of；10 Limitations nologies, Volume 1 (Long and Short Papers), pages
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Automatic Prompt Augmentation and Selection with Chain-of-Thought from Labeled Data”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
