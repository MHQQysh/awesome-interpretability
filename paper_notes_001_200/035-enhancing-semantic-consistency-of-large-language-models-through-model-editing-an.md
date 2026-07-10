# 035. Enhancing Semantic Consistency of Large Language Models through Model Editing: An Interpretability-Oriented Approach

> 逐篇阅读记录：第 35 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jingyuan Yang, Dapeng Chen, Yajing Sun, Rongjun Li, Zhiyong Feng, Wei Peng
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.199)
- **领域标签**：Evaluation, Hidden, LLM, Activation, Control
- **本地 PDF 文本规模**：约 6,888 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：A Large Language Model (LLM) tends to generate inconsistent and sometimes contradictory outputs when presented with a prompt that has equivalent semantics but is expressed differently from the original prompt.To achieve
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction engineering or data-driven methods to handle the；2 Related Work；3 Preliminary；5 NLU Benchmark Construction In specific, we first use the left 6 task instruc-；7 Conclusion Askell, Sandhini Agarwal, Ariel Herbert-Voss,；2022. Training language models to follow instruc-
- **引言关键线索**：problem of semantic consistency. For example, The field of Natural Language Processing (NLP) is Raj et al. (2023) proposed a prompt strategy called experiencing a paradigm shift with the advent of 鈥楢sk-to-Choose鈥 (A2C) to improve the semantic Large Language Models (LLMs). These models consistency of LLMs, but this method requires care- have demonstrated remarkable capabilities in var- fully designed prompts. Applying a data-driven su- ious tasks such as sentiment classification (Wang pervised fine-tuning method (SFT) (Ouyang et al., et al., 2023), machine translation (Hendy et al., 2022) to finetune an LLM with prompt-output pairs * Corresponding Author with semantically equivalent meanings is another 3343 Findings of the Association for Computational Linguistics: ACL 2024, pages 3343鈥3353 August 11-16, 2024 漏2024 Association for Computational Linguistics effective approach. Despite thei
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Jingyuan Yang1,2 , Dapeng Chen2 , Yajing Sun2 , Rongjun Li2 , Zhiyong Feng1, * and Wei Peng2, * 1 College of Intelligence and Computing, Tianjin University 2 IT Innovation and Research Center, Huawei Technologies {yangjingyuan2, chendapeng8, sunyajing4, lirongjun3, peng.wei1}@huawei.com zyfeng@tju.edu.cn Abstract Prompt1 What sport does Luis Eduardo Maldonado play? Large Prompt1 AWhat Language sport (LLM) tends ModelMaldonado does Luis Eduardo play? to gen- baseball erate inconsistent and sometimes contradictory outputs when presented baseball with a prompt that has inconsistency equivalent semantics in-consistency but is expressed differ- football ently from the original prompt. To achieve football Prompt2 Luis Eduardo Maldonado plays what sport? semantic consistency of an LLM, one of the Prompt2 key approaches is to finetune Luis Eduardo Maldonado model with the sport? plays what What sport does Luis Eduardo Maldonado play? Prompt1 pr
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Hidden, LLM, Activation, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The, Comparison with the SFT, Method, Table 8, Comparison of performance and, IMDB for movie reviews, Table 9, Comparison of the performance, Table 10, The comparison of the, Table 11
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - as in our editing method. The 鈥渞andom direction鈥 is the 6.5.5 Comparison with the SFT Method
  - Table 8: Comparison of performance and consistency
  - et al., 2015), IMDB for movie reviews senti- Table 9: Comparison of the performance and con-
  - Table 10: The comparison of the computational cost
  - Table 11: Comparison of performance and semantic standing intermediate layers using linear classifier

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：23 times, 0.84 points, 18X, 23X, 12X, 19X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - datasets, our method demonstrates significant tic consistency鈥 (Gan and Mori, 2023; Rabinovich
  - improvements in the semantic consistency and et al., 2023; Raj et al., 2022), largely limiting the
  - experiencing a paradigm shift with the advent of 鈥楢sk-to-Choose鈥 (A2C) to improve the semantic
  - consistency. We subsequently inject biases into the Elazar et al. (2021) revealed significant semantic
  - structed relevant NLU-task datasets in addition to spite the significant shift in the Natural Language
  - Our method has shown significant enhancements driven SFT. For example, Raj et al. (2023) proposed
  - parameters, resulting in a significant saving three types of editing methods: external memory-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：of the top-K model components can significantly to a decline in model performance. It is noted that enhance the model鈥檚 semantic consistency and task at 伪 = 3.0, the LLM achieves better results than performance. Importantly, these advancements are those reported in Table 2, highlighting the need to achieved without the need for altering the model鈥檚 optimize the value of 伪 in practice. underlying parameters. Method RobustSST2 6.5 Ablation Study LLama2-7B 85.66卤4.88 6.5.1 The Influence of Hyperparameter K +Editing (伪=1.0) 89.90卤4.54 +Editing (伪=3.0) 89.90卤2.98 We investigate the impact of the K-value +Editing (伪=5.0) 89.90卤4.54 on the experimental setting, selecting the Ro- +Editing (伪=7.0) 88.53卤5.09 +Editing (伪=9.0) 83.48卤7.50 bustSST2 dataset for our analysis, with the k 鈭 {5, 15, 25, 35, 45, 55}. As demonstrated in Figure Table 4: The performance of the proposed model editing 4, it is observed that the edited model achieves the method under different 伪 values. highest accuracy when t
- **局限/未来工作线索**：To address the limitations of previous methods；investigation of this approach in future work.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Enhancing Semantic Consistency of Large Language Models through Model Editing: An Interpretability-Oriented Approach”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
