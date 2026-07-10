# 064. Large Language Models Are Reasoning Teachers

> 逐篇阅读记录：第 64 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Namgyu Ho, Laura Schmid, Se-Young Yun
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.acl-long.830)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 20,615 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Recent works have shown that chain-of-thought (CoT) prompting can elicit language models to solve complex reasoning tasks, step-by-step.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction LLMs to perform chain-of-thought (CoT) reason-；2 Related Work teacher models that are inaccessible to the general；4 Experiments；8 Ethics Statement References；2015. Parsing algebraic word problems into equa- Maxwell Nye, Anders Johan Andreassen, Guy Gur-Ari,；2021. Show your work: Scratchpads for intermediate cloze-questions for few-shot text classification and；2021. Are nlp models really able to solve；2020. Automatically identifying words that can serve proves chain of thought reasoning in language mod-
- **引言关键线索**：ing, i.e., generate a series of intermediate reason- Language models (LMs) have demonstrated re- ing steps. This can be achieved by providing CoT markable performance in a wide range of down- demonstrations as exemplars in prompting (Wei stream tasks. Recently, large language models et al., 2022b). More recently, Kojima et al. (2022) (LLMs) have demonstrated in-context generaliza- found that LLMs can be prompted to perform CoT tion capabilities: performing downstream tasks reasoning simply by providing a natural language simply by conditioning on few in-context exem- instruction to think step-by-step. plars or plain natural language task descriptions A major drawback of prompt-based CoT rea- (Brown et al., 2020; Sun et al., 2021). Despite soning methods, however, is their reliance on ex- these advancements, even the largest LLMs have tremely large models that span hundreds of billions be
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：pendent on very large models such as GPT-3 Zero-shot-CoT 175B which are prohibitive to deploy at scale. In this paper, we use these large models as rea- Reasoning samples soning teachers to enable complex reasoning in smaller models and reduce model size require- ? ments by several orders of magnitude. We pro- ? ? pose Fine-tune-CoT, a method that generates reasoning samples from very large teacher mod- els to fine-tune smaller models. We evaluate our method on a wide range of public models Small and complex tasks. We find that Fine-tune- Fine-tuning Student CoT enables substantial reasoning capability in small models, far outperforming prompt-based baselines and even the teacher model in many Figure 1: Fine-tune-CoT uses teacher-generated rea- tasks. Additionally, we extend our method by soning to teach students. We prompt a very large leveraging the teacher model鈥檚 ability to gener- teacher model, such as GPT-3 175B, to solve complex 
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Figure 1, Fine-tune-CoT uses teacher-generated rea-, Baseline methods We provide, Fine-tune-CoT and diverse, Fine-tune-CoT, We compare with various, CoT, CoT baselines as 10, Baseline per-, Few-shot-CoT are shown for, Diverse, On the other hand, Fine-tune-CoT elic- reasoning is, Few-shot-
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - baselines and even the teacher model in many Figure 1: Fine-tune-CoT uses teacher-generated rea-
  - diverse reasoning) and baseline methods. 鈥楻andom鈥 refers to random-guess performance derived based on the
  - Baseline methods We provide a comparison of mance of models using Fine-tune-CoT and diverse
  - Fine-tune-CoT (ours) with four baseline methods: reasoning. We compare with various baselines and
  - CoT, compared to prompt-based CoT baselines as 10
  - els, showing near-negligible performance across varying degrees of diverse reasoning D. Baseline per-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：8 times, 0.2 times, 44.00%, 11.67%, 44 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and complex tasks. We find that Fine-tune- Fine-tuning Student
  - small models, far outperforming prompt-based
  - Fine-tune-CoT, which utilizes the reasoning capa- data for student models3 . We find that this is a
  - dard fine-tuning without rationales has been shown wide range of publicly available models. We find
  - downstream tasks without hand-crafted reasoning ables models as small as 0.3B to outperform larger
  - CoT prompting method in Section 7.3. In the fol- to significantly boost reasoning supervision sim-
  - significant gains over Zero-shot-CoT when using

## 6. Conclusion、局限与可复现性

- **结论段落线索**：performance boost of Fine-tune-CoT when using diverse reasoning with D = 64 on MultiArith and SVAMP; 5.1 Accessibility of Fine-tune-CoT D = 8 on others. Owing to the versatility of the teacher generation method, i.e., Zero-shot-CoT, our method can be readily applied to any complex task without task- 4.2 Analysis specific engineering. Rationales can be readily gen- Sample study To identify the strengths and weak- erated using publicly available APIs such as those nesses of our method, we perform a thorough sam- provided by OpenAI or Anthropic. This makes it ple study across all datasets and methods. Across viable to obtain CoT training data in low-resource all arithmetic tasks, we find that a large portion scenarios, which not only outperforms standard of errors arises from calculations. MultiArith and fine-tuning, but elicits the student to output inter- SVAMP also show many semantic errors, but these pretable explanations. Fine-tuning and inference are significantly reduced with diver
- **局限/未来工作线索**：leave further analysis to future work. tune-CoT (with diverse reasoning) in the frequency；7 Limitations et al., 2022). However, our choice to use Zero-；promising for future work. comes to the ability of the student model to gener-；performance, such as using different prompting be of interest for future work to formalize the con-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Large Language Models Are Reasoning Teachers”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
