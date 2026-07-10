# 124. Don’t Just Say “I don’t know”! Self-aligning Large Language Models for Responding to Unknown Questions with Explanations

> 逐篇阅读记录：第 124 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yang Deng, Yong Zhao, Moxin Li, See-Kiong Ng, Tat-Seng Chua
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.757)
- **领域标签**：Rationale, Evaluation, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 14,195 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Despite the remarkable abilities of Large Language Models (LLMs) to answer questions, they often display a considerable level of overconfidence even when the question does not have a definitive answer.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；4 Experimental Setups performs multi-class classification to categorize；5. All responses are generated using Vicuna as the and to proactively respond to them with appropri-；6 Conclusions Restricted Applicability to Black-box Large
- **引言关键线索**：et al., 2023a) or knowledge-enhanced techniques Large Language Models (LLMs) have showcased (Shen et al., 2018; Deng et al., 2018, 2022; Asai exceptional capabilities in performing high-quality et al., 2023; Jiang et al., 2023) to improve the ac- conversational information seeking, even when en- curacy of the responses. Despite the improvement countering user questions that require complex on correctly answering those known questions that reasoning (Wei et al., 2022) or extensive external have definitive answers but a specific model may knowledge (Yao et al., 2023b). However, LLMs not know, LLMs still tend to assertively respond tend to exhibit a significant degree of overconfi- to questions that do not have a definitive answer, dence (Si et al., 2023; Mielke et al., 2022) when i.e., objectively unanswerable. Trustworthy and answering the questions that they are aware of. This reliable L
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Self-aligned to enhance its response-ability to different types does not feature an animal at the top. Answer Instead, the trophy is topped by a of unknown questions, being capable of not silver cup with a pineapple-like design. just refusing to answer but further proactively fi providing explanations to the unanswerability of unknown questions. Specifically, the Self- Figure 1: Comparisons of different types of responses Align method first employ a two-stage class- to an unknown question that contains incorrect assump- aware self-augmentation approach to generate tion. Red words denote the hallucinated content, while a large amount of unknown question-response underlined word denotes the explanation. data. Then we conduct disparity-driven self- curation to select qualified data for fine-tuning the LLM itself for aligning the responses to definitive answer, potentially leading to hallucina- unknown questions as desired. Experimental res
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Specifically, Self- Figure 1, Comparisons of different types, Self-Aligned method over existing, To mitigate the hallucination, Deng et al, Baselines ods appears to, Def, Zero-shot, However, Ap- Remarkably, Vicuna, F1 score in this, Table 2. Among the, Automatic Evaluation
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - of unknown questions. Specifically, the Self- Figure 1: Comparisons of different types of responses
  - Self-Aligned method over existing baselines in To mitigate the hallucination issue, existing stud-
  - lacks a definitive answer (Deng et al., 2023a, 2024). baselines in terms of three types of task formula-
  - performance of the prompt-based baseline meth-
  - 4.4 Baselines ods appears to be unreliable, exhibiting instabil-
  - baselines for comparisons, including three prompt- the Def+q鈥(5)+q(5) method largely depends on the

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：320
            X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - exceptional capabilities in performing high-quality et al., 2023; Jiang et al., 2023) to improve the ac-
  - conversational information seeking, even when en- curacy of the responses. Despite the improvement
  - tend to exhibit a significant degree of overconfi- to questions that do not have a definitive answer,
  - pioneer studies, there are several issues that remain approach to utilize LLMs to improve its own
  - stead of studying the uncertainty of model knowl- ditional context to improve the output at inference
  - the improved model M(1) . In general, the base classification for known and unknown questions.
  - no significant performance improvement after that. 5.2 Unknown Question Classification

## 6. Conclusion、局限与可复现性

- **结论段落线索**：that the Self-Aligned method fails to generate more 5.4.1 Effect of Self-Curation comprehensive responses than the Hint method, In order to validate the effectiveness of the pro- which also leads to the higher automatic scores posed Disparity-driven Self-curation, we conduct assessed by GPT-4 in Section 5.3.1. the analysis of the effect of self-curation strategies. From the perspective of three evaluation criteria, We compare to two variants of our Self-Aligned the model鈥檚 score for Honesty is generally higher Method as follows: than that for Comprehensibility. This indicates that, despite providing honest answers to some ques- 鈥 No Self-curation: We directly conduct super- tions, the model fundamentally does not accurately vised fine-tuning over the self-augmented un- 13658 QNotA KUQP Model Self-Aligned (K=3) vs. Method Incomp. Future Incorr. Ambig. Avg Incomp. Future Incorr. Ambig. Avg Zero-shot 0.563 0.575 0.525 0.713 0.594 0.563 0.600 0.638 0.588 0.597 Few-shot (5) 0.638 0.725 0.62
- **局限/未来工作线索**：ity of recognizing its own knowledge limitations iterations, which indicates the effectiveness of it-；which is an incorrect question, the incorrectness Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Don’t Just Say “I don’t know”! Self-aligning Large Language Models for Responding to Unknown Questions with Explanations”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
