# 062. Revisiting Relation Extraction in the era of Large Language Models

> 逐篇阅读记录：第 62 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Somin Wadhwa, Silvio Amir, Byron Wallace
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.acl-long.868)
- **领域标签**：Rationale, Reasoning, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 13,166 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：models; (2) Flan-T5 is not as capable in the few-shot setting, but supervising and fine-tuning it with Chain-of-Thought (CoT) style explanations (generated via GPT-3) yields SOTA results.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3. Evaluating the performance of generative mod-；2 RE via Text Generation Challenges inherent to evaluating generative；3 In-Context Few-Shot Learning with work on generative RE over CoNLL, we use the；2. Few-shot；4 SOTA RE Performance with Flan-T5 CoNLL The prompt for CONLL consisted of；2010. Modeling relations and their mentions without
- **引言关键线索**：Relation extraction (RE) is the task of identifying yields near SOTA performance on standard RE entities and their semantic relationships from texts. datasets, outperforming fully supervised models. Standard supervised approaches (Eberts and Ulges, 2. We find that Flan-T5 (large; Chung et al. 2022) 2019a) to RE learn to tag entity spans and then is not as capable, even when fine-tuned. But we classify relationships (if any) between these. More then propose an approach to training Flan-T5 with recent work has shown that conditional language Chain-of-Thought (CoT) style 鈥渆xplanations鈥 (gen- models can capably perform this task鈥攁chieving erated automatically by GPT-3) that support rela- SOTA or near-SOTA results鈥攚hen trained to out- tion inferences; this achieves SOTA results. put linearized strings encoding entity pairs and
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Figure 1: RE performance of LLMs on the CoNLL (GPT-3 and Flan-T5 large) than considered in dataset. 1 Few-shot GPT-3 slightly outperforms the ex- prior work and evaluating their performance isting fully supervised SOTA method (Huguet Cabot and on standard RE tasks under varying levels of Navigli 2021; dotted horizontal line). 2 Eliciting CoT supervision. We address issues inherent to eval- reasoning from GPT-3 further improves few-shot per- uating generative approaches to RE by doing formance. 3 Fine-tuning Flan-T5 (large) is competitive human evaluations, in lieu of relying on exact with, but no better than, existing supervised methods, matching. Under this refined evaluation, we but 4 supervising Flan-T5 with CoT reasoning elicited find that: (1) Few-shot prompting with GPT-3 from GPT-3 substantially outperforms all other models. achieves near SOTA performance, i.e., roughly equivalent to existing fully supervised mod- els; (2) Flan-T
- **方法与解释性关系**：该论文主要围绕 `Rationale, Reasoning, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We release this model, GPT-3, Brown et al, To ensure fair comparison, Table 2, Comparison of, SpERT, Relation, To generate the targets, Comparisons in End-to-end Relation, Extraction, In tion for Computational, Linguistics
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - sults. We release this model as a new baseline language models鈥-including GPT-3 (Brown et al.,
  - was 960 tokens. To ensure fair comparison to prior
  - Table 2: Comparison of (micro-F1) performance with recent generative (except SpERT) approaches in RE. Relation
  - to train compared with existing fully supervised To generate the targets in this case, we start with
  - should be a standard baseline for RE.
  - Comparisons in End-to-end Relation Extraction! In tion for Computational Linguistics, pages 764鈥777,

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：9.97 point, 3.37 point, 10 points, 143 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - dataset. 1 Few-shot GPT-3 slightly outperforms the ex-
  - reasoning from GPT-3 further improves few-shot per-
  - from GPT-3 substantially outperforms all other models.
  - 1 Introduction 1. We show that few-shot learning with GPT-3
  - entities and their semantic relationships from texts. datasets, outperforming fully supervised models.
  - Standard supervised approaches (Eberts and Ulges, 2. We find that Flan-T5 (large; Chung et al. 2022)
  - Our results indicate that, in general, LLMs should NYT 4 24 56,196 5,000 5,000

## 6. Conclusion、局限与可复现性

- **结论段落线索**：lect human annotations judging the model鈥檚 gener- exemplars capturing all entity and relation types. ations against the gold references. Finally, using The size of this prompt was 2095 tokens. these annotations we report results achieved using GPT-3 with few-shot prompting for RE (Table 2). We next aim to evaluate the performance of GPT- All references to GPT-3 in this work refer to the 3 for RE when provided the above prompts. But 鈥渢ext-davinci-002鈥 variant. doing so requires addressing the challenges inher- ent to evaluating LLMs for RE outlined above (and 3.1 Prompts in prior work; Taill茅 et al. 2020). We describe the prompts we use for each of the datasets considered in turn. 3.2 Manually re-evaluating 鈥渆rrors鈥 We quantify the errors in evaluation that occur ADE To construct prompts for ADE, we use the when one uses 鈥渟trict鈥 measures of performance instructional prompt: List all (drug: adverse ef- while using few-shot prompted LLMs for RE fects) pairs in the following text, followe
- **局限/未来工作线索**：can be seen in Table 2 (2.a). We also find a substan- maining limitation of in-context learning with large；Despite these limitations, the fact that GPT-3 is；Limitations notations ourselves to determine fair approximate；But there are important limitations to these the National Science Foundation (NSF) grant III-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Revisiting Relation Extraction in the era of Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
