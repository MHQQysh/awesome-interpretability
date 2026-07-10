# 156. LogiCoT: Logical Chain-of-Thought Instruction Tuning

> 逐篇阅读记录：第 156 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Hanmeng Liu, Zhiyang Teng, Leyang Cui, Chaoli Zhang, Qiji Zhou, Yue Zhang
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.191)
- **领域标签**：Rationale, Evaluation, Reasoning, Behavior, Analyze
- **本地 PDF 文本规模**：约 11,780 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Generative Pre-trained Transformer 4 (GPT-4) demonstrates impressive chain-of-thought reasoning ability.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction Hypothetical Syllogism:
- **引言关键线索**：From the fact that it is cloudy if and only if Jessica is playing a game we can paraphrasing. However, they fall short of help- infer that if Jessica plays a game, then it is cloudy via biconditional elimination. Finally, from the fact that if it is late, then Jessica is playing a game, and that if ing the model handle complex reasoning tasks. Jessica plays a game, then it is cloudy we can infer that if it is late, then it is cloudy via transitivity, which contradicts that The fact that it is late does not To bridge the gap, this paper presents LogiCoT, imply that it is cloudy. a new instruction-tuning dataset for Logical Therefore, the answer is no. GPT-4 reasoning step by step: Chain-of-Thought reasoning with GPT-4. We elaborate on the process of harvesting instruc- Given the premises:
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：marks indicate remarkable performance elevations large language models (LMs). The core innova- compared to state-of-the-art instruction-tuned mod- tion lies in its prompt-based learning combined els, showing the promise of leveraging complex with counterfactual regularization to ensure faith- CoT instruction data in the instruction-tuning pro- fulness in the reasoning over generated rationales. cess of LLMs. While PINTO excels in achieving superior perfor- Our work is in line with recent research indicat- mance across in-distribution and out-of-distribution ing that smaller language models can achieve com- test sets and ensuring that rationales are faithful, petitive multi-step reasoning abilities when special- its primary motivation is different from LogiCoT. ized on targeted CoT tasks. Examples of these tasks Whereas PINTO emphasizes rationale transparency include the execution of SQL commands, mathe- and faithfulness, LogiCoT is gear
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：To better understand the
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - baselines. To better understand the specific contributions

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：10 points, 20 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - embodying the ability to infer conclusions based weights.
  - 2023a) presents a significant gap, inhibiting the
  - soning, represents a significant gap in the current
  - strated marked improvements in various reasoning which is illustrated in Figure 1. We first establish a
  - tions for each premise and conclusion, which are formal logic, predicting possible inferences from
  - eration quality. vance, we acknowledge room for improvement in
  - Geography 28.8 52.5 improvement is salient given that our model has

## 6. Conclusion、局限与可复现性

- **结论段落线索**：on a structured progression of premises. The dearth of such abilities in community models (Liu et al., 2 Related Work 2023a) presents a significant gap, inhibiting the Instruction tuning LLMs. Instruction-tuning of development of open LLMs with strong chain rea- Large Language Models (LLMs) has become a soning. While GPT-4 has demonstrated its ability thriving research area in Natural Language Process- to produce high-quality CoT reasoning output, the ing (NLP), aiming to enable zero-shot generaliza- potential of generating CoT instruction tuning data tion on unseen tasks (Zhong et al., 2021; Ouyang using this model remains largely unexplored. The et al., 2022; Wei et al., 2022). This involves fine- need to cover more diverse and complex reason- tuning LLMs to perform diverse tasks by following ing scenarios, particularly multi-step logical rea- a set of instructions, making the task source an es- soning, represents a significant gap in the current sential component of instruction tuni
- **局限/未来工作线索**：The machine reading comprehension task is de- faithfulness, and future work will aim to refine the；datasets used. Future research can delve deeper enhance a generative LLM. Future work includes
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“LogiCoT: Logical Chain-of-Thought Instruction Tuning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
