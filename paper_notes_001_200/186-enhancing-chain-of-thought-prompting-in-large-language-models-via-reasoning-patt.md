# 186. Enhancing Chain of Thought Prompting in Large Language Models via Reasoning Patterns

> 逐篇阅读记录：第 186 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yufeng Zhang, Xuepeng Wang, Lingxiang Wu, Jinqiao Wang
- **发表 venue / date**：AAAI / 2025/04
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/34793/36948)
- **领域标签**：Analysis, Reasoning, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 6,903 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Chain of Thought (CoT) prompting can encourage language models to engage in multi-step logical reasoning.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：5840. Toronto, Canada: Association for Computational Lin-
- **引言关键线索**：prompt influences how LLMs arrive at the final answer. Large Language Models (LLMs) have demonstrated excep- tional performance across a wide range of language tasks. In general question-answering tasks (Kwiatkowski et al. 2019), LLMs hold a distinct advantage over other language mod- tasks, we can route the model to the appropriate context, and els due to their robust writing capabilities. However, when then easily switch the demonstration sets to activate the rel- it comes to more advanced tasks such as logical reasoning, evant knowledge and abilities in the corresponding domain. mathematical computation, and symbolic reasoning, LLMs However, we argue that existing unsupervised CoT often fall short (Qiao et al. 2023; Huang and Chang 2023). prompting methods have two major shortcomings. First, One effective approach to addressing these challenges there remains a significant gap between 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tics of the questions, which can introduce noise and lack in- terpretability. In this paper, we propose leveraging reasoning patterns to enhance CoT prompting effectiveness. Reasoning patterns represent the process by which language models ar- rive at their final results. By utilizing prior knowledge and prompt-based methods from large models, we first construct task-specific pattern sets. We then select diverse demonstra- tions based on different reasoning patterns. This approach not only mitigates the impact of noise but also provides explicit interpretability to help us understand the mechanisms of CoT. Extensive experiments demonstrate that our method is more robust and consistently leads to improvements across various reasoning tasks. Introduction Figure 1: Example of the chain-of-thought prompting. The prompt influences how LLMs arrive at the final answer. Large Language Models (LLMs) have demonstrated excep- tional performance ac
- **方法与解释性关系**：该论文主要围绕 `Analysis, Reasoning, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：LLMs engage in ICL, Even if all the, For tasks with, Baselines, We primarily compare our, Additionally, Figure 3, Comparison of different operation
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - crucial when LLMs engage in ICL. Even if all the demon- tions, square roots, comparison symbols, etc. For tasks with
  - Baselines. We primarily compare our methods with unsu-
  - the baseline in experiments. Additionally, we conduct ex-
  - Figure 3: Comparison of different operation sets.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：10 x, 7 x, 8 times, 8 x, 0.8 times, 000 x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - ity of the provided demonstrations significantly influences the
  - robust and consistently leads to improvements across various
  - One effective approach to addressing these challenges there remains a significant gap between the selected demon-
  - To better select a demonstration subset for a reasoning Large language models have demonstrated significant abil-
  - and (Madaan, Hermann, and Yazdanbakhsh 2023), we ob- highlight that LLMs can achieve improved task completion
  - task-specific knowledge, our method improves interpretabil-
  - CoT patterns and show how they can improve the reasoning

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tives. This paper aims to address the noise issue inherent in unsu- pervised semantic-based CoT methods and proposes a rea- Error Robustness (RQ3) soning pattern-based approach for CoT demonstration selec- It is worth mentioning that we do not impose supervision on tion. Our method explicitly enhances the interpretability of the labels of the demonstrations. Therefore, we proceed to reasoning processes and illustrates how LLMs can be guided count the number of incorrect instances within the selected toward generating accurate answers. Extensive experiments set, as shown in Table 4. It is intriguing to notice that the validate the effectiveness, robustness, and compatibility of majority of our provided demonstrations are imperfect, with our approach. 25991 Acknowledgements Levy, I.; Bogin, B.; and Berant, J. 2023. Diverse Demon- This work was supported by the National Key R&D strations Improve In-context Compositional Generalization. Program of China (Grant No.2023ZD0120400), Bei- In Ro
- **局限/未来工作线索**：ther optimization. This limitation can be particularly prob- Chain-of-Thought Prompting
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Enhancing Chain of Thought Prompting in Large Language Models via Reasoning Patterns”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
