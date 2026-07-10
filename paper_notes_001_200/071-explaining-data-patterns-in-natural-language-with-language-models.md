# 071. Explaining Data Patterns in Natural Language with Language Models

> 逐篇阅读记录：第 71 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Chandan Singh, John X. Morris, Jyoti Aneja, Alexander M. Rush, Jianfeng Gao
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.blackboxnlp-1.3)
- **领域标签**：Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 14,463 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Large language models (LLMs) have displayed an impressive ability to harness natural language to perform complex tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related work related work directly finetunes an LLM to describe；3 Methods: Defining the task and；1) Synthetic math 10 Mathematical functions (i), (ii)；2) Allen NLI 10 Language tasks (i), (ii)；4) Sentiment 4 Sentiment classification (i), (ii) Add；5) Proteins/chemicals 3 Protein/chemical properties (iii)；6) Language fMRI 20 Excitation of fMRI voxel (iii),(iii) Subtract；4 Experimental Setup
- **引言关键线索**：2021; Deng et al., 2022; Zhou et al., 2022). Here, Large language models (LLMs) have attained an we study interpretable autoprompting (iPrompt), extraordinary ability to harness natural language which aims to find a semantically meaningful nat- for solving diverse problems (Devlin et al., 2018), ural language prompt that explains a key charac- often without the need for finetuning (Brown teristic of the data. For example, given a dataset et al., 2020; Sanh et al., 2021). Moreover, LLMs of examples of addition, e.g. 2 5 鈬 7 ... 3 1 鈬 have demonstrated the capacity to excel at real- 4, iPrompt yields the natural language explanation world problems, such as mathematics (Lewkowycz Add the inputs (see Fig. 1). iPrompt works by us- et al., 2022), scientific question answering (Sa- ing a pre-trained LLM to iteratively propose and dat and Caragea, 2022), predicting brain re- evaluate different c
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tions that are human-interpretable. Moreover, iPrompt is reasonably efficient, as it does not prompting. Prompting has emerged as an effective require access to model gradients and works method for adapting LLMs to new datasets (Liu with relatively small models (e.g. 6 billion et al., 2021a); a prompt string is combined with parameters rather than 鈮100 billion). Finally, each example in a dataset before querying an LLM experiments with scientific datasets show the for an answer. While prompts were initially con- potential for iPrompt to aid in scientific discov- structed manually, recent work has shown success ery. 1 in autoprompting, automatically finding a prompt
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：LLM to discover and, Baseline, AutoPrompt AutoPrompt, Shin et al, Zero-shot suffix decoding LLMs, Figure 4, Comparison of model accuracy, Human evaluation scores are, PhD stu- baselines and, As we have examples, Results are averaged over, When its prompts are, GPT-3, AutoPrompt only slightly outperforms, For fair comparison, AutoPrompt, We generate, Galactica proposals from empty
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - age the learned skills of an LLM to discover and performs baseline methods in accurately finding
  - prompt s and each example in the dataset (xi , y i ). Baseline: AutoPrompt AutoPrompt (Shin et al.,
  - Baseline: Zero-shot suffix decoding LLMs
  - Figure 4: Comparison of model accuracy with correct
  - Human evaluation scores are averaged over 4 PhD stu- baselines and approach the accuracy of the cor-
  - prompts. As we have examples of the task, we better than for the Baseline (which again contains

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - In this work, we probe whether we can lever- a ground-truth pattern. We find that iPrompt out-
  - tion datasets, Finally, we find that iPrompt is able effectively alter a model鈥檚 predictions, but do not
  - that improve supervised prediction (Honovich et al., extractive rationales (Zaidan and Eisner, 2008; Sha
  - often yielding impressive improvements in perfor- ing (Conneau et al., 2018; Liu and Avci, 2019) or by
  - Liu et al., 2021b) to improve the process of prompt-
  - s虃. It discretely changes individual words in s虃 and (ii) improves the score of the prompts on the ob-
  - improves the objective score. The use of gradients running inference on the pre-trained LLM, yield-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：pendix A.4). details and Appendix A.3 for experiments on more 34 Table 2: Performance for dataset explanation. Dataset 1.0 Prompt Recovery (MRR) from Table 1 (1-3). Accuracy measured via (1) Human- evaluation (H, normalized %), (2) Mean Reciprocal 0.8 Rank across the collection (M) and (3) 1-best correct- 0.6 ness (C, %). For all metrics, higher is better. 0.4 iPrompt AutoPrompt Suffix 0.2 Math H/M/C H/M/C H/M/C ANLI 0.0 Math 60 / 0.69 / 60 25 / 0.14 / 13 20 / 0.08 / 03 0 20 40 60 80 100 ANLI Induction 56 / 0.41 / 37 42 / 0.35 / 28 21 / 0.07 / 07 21 / 0.09 / 08 25 / 0.06 / 01 23 / 0.04 / 01 Model accuracy with correct prompt Figure 4: Comparison of model accuracy with correct prompt and iPrompt ability to find the correct prompt models (e.g. Galactica (Taylor et al., 2022)) and across each individual task (single-task MRR). Prompt recovery ability is dependent on the model鈥檚 ability to more datasets. perform the task. Evaluation metrics Our main evaluation mea- sures each prompt鈥檚 clos
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Explaining Data Patterns in Natural Language with Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
