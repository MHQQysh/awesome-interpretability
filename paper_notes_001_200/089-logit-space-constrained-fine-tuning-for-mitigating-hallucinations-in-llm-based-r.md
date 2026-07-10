# 089. Logit Space Constrained Fine-Tuning for Mitigating Hallucinations in LLM-Based Recommender Systems

> 逐篇阅读记录：第 89 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jie Deng, Qingfeng Chen, Debo Cheng, Jiuyong Li, Lin Liu
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.1491)
- **领域标签**：Representation, Evaluation, Hallucination, Hidden, Behavior, Detect
- **本地 PDF 文本规模**：约 9,070 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Large language models (LLMs) have gained increasing attention in recommender systems, but their inherent hallucination issues significantly compromise the accuracy and reliability of recommendation results.Existing LLM-b
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Related Work；3 Preliminary；2024 IEEE 40th International Conference on Data the most recent 20 months of interaction records
- **引言关键线索**：(Dai et al., 2023b) integrates LLMs with traditional Large language models (LLMs), such as GPT- recommendation models to enhance performance. 3 (Ouyang et al., 2022; Brown et al., 2020) and However, due to the discrepancy between the pre- LLaMA (Touvron et al., 2023), are pretrained on training objectives of LLMs and the specific goals massive datasets and demonstrate exceptional ca- of recommendation tasks, using LLMs directly as pabilities in contextual understanding, knowledge recommendation generators still faces performance reasoning, and compositional generalisation. Their limitations in practical applications. utility has expanded beyond traditional natural To mitigate the discrepancy between the pre- language processing (NLP) (Liang et al., 2022; training objectives of LLMs and the goals of rec- Chowdhery et al., 2022; Wei et al., 2022) to fields ommendation tasks, several studie
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：hallucination issues during the fine-tuning pro- cantly (Chen et al., 2024; Zhang et al.; Deng et al., cess. To address this challenge, we propose 2024). LLMs can mitigate cold-start problems for Logit Space Constraints Fine-Tuning (LCFT), users and items by leveraging their extensive pre- a novel fine-tuning framework designed to mitigate hallucination in LLM-based recom- trained knowledge (Sanner et al., 2023), support dy- menders. Specifically, LCFT takes as input namic modelling through contextual reasoning (Dai semantically positive and negative instruction et al., 2023a; Gao et al., 2023), and introduce novel pairs and incorporates Kullback鈥揕eibler (KL) mechanisms to transcend the limitations of tradi- divergence into the training objective to ex- tional collaborative filtering approaches. plicitly maximise their distributional disparity Recommendation methods based on LLMs have in the logit space. By conducting such logit demonst
- **方法与解释性关系**：该论文主要围绕 `Representation, Evaluation, Hallucination, Hidden, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：LKL represents the training, Baselines, To verify the effectiveness, LCFT-TALLRec over baseline methods, LCFT-CoLMF over the baseline, Appendix B. achieves the, Performance Comparison tion space, LLMs, The table 1 presents, Although LLM-based, Compared with various baseline
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - ing setting, then describe the baseline methods and
  - tuning objective, and LKL represents the training Baselines. To verify the effectiveness of our pro-
  - the average relative improvement of LCFT-TALLRec over baseline methods across the three different shot settings.
  - and compare it with the following baseline meth- between users and items via inner product. (8)
  - denotes the average relative improvement of LCFT-CoLMF over the baseline methods on the two evaluation metrics.
  - found in Appendix B. achieves the best performance among all baselines,

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：10 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - their inherent hallucination issues significantly
  - dation datasets to fine-tune LLMs and improve of LLM-based recommender systems can signif-
  - models and applies joint fine-tuning to improve tem does not suffer from hallucination and pos-
  - ment learning to optimise LLMs, significantly en- generate different recommendation results when
  - to improve ranking accuracy; (Zheng et al., 2024) based recommender systems, we propose a novel
  - mitigates hallucinations and improves recommen- their potential in recommender systems. A repre-
  - In this section, we review several studies related to tion scenario-specific instructions to improve rec-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：(RQ3) and hyperparameter analyses (RQ4), with detailed analysis provided in Appendices C and D, In this work, we introduced LCFT, a novel fine- respectively. tuning framework designed to mitigate hallucina- tion in LLM-based recommender systems. LCFT mitigates hallucination by taking semantically op- 5.3 Case Studies of LCFT posite instruction pairs as input and incorporates a KL divergence term into the training objective to We present case studies from the MovieLens-100K enlarge their distributional differences in the logit dataset (a movie watching scenario) in Table 3. space. Extensive experiments conducted on two These examples demonstrate that traditional fine- LLM-based recommender systems built on differ- tuning methods can yield logically inconsistent or ent LLM backbones and across four real-world contradictory outputs. For example, in the case datasets demonstrate that LCFT effectively reduces of user1, a baseline model simultaneously infers hallucinations and improves recom
- **局限/未来工作线索**：pairs and incorporates Kullback鈥揕eibler (KL) mechanisms to transcend the limitations of tradi-；reasoning, and compositional generalisation. Their limitations in practical applications.；鈥 Extensive experiments on LLM-based recom- To overcome the limitations of directly using LLMs；they still show limitations in modelling complex et al., 2024; Zhu et al., 2024; Hong et al., 2025;
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Logit Space Constrained Fine-Tuning for Mitigating Hallucinations in LLM-Based Recommender Systems”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
