# 033. UHGEval: Benchmarking the Hallucination of Chinese Large Language Models via Unconstrained Generation

> 逐篇阅读记录：第 33 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xun Liang, Shichao Song, Simin Niu, Zhiyu Li, Feiyu Xiong, Bo Tang, Yezhaohui Wang, Dawei He, et al.
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.288)
- **领域标签**：Evaluation, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 14,838 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Xun Liang, Shichao Song, Simin Niu, Zhiyu Li, Feiyu Xiong, Bo Tang, Yezhaohui Wang, Dawei He, Cheng Peng, Zhonghao Wang, Haiying Deng.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction where hallucinations arise in unrestricted, sponta-；1 Data Collection and Pre-processing 2 Unconstrained Hallucination Generation 3 Hallucination Ranking；5 Automated Evaluation 4 Automatic Labeling And Human Recheck；2 Related Work；1 Hallucination Elements Extraction Figure 5: Evaluation framework；3 Re-check By Human；4 Experiments UHGEval is both intuitive and secure for users,；2024. Controlled text generation for large language
- **引言关键线索**：Large language models (LLMs) have unparalleled neously generated content. For example, HaluEval proficiency in language generation, knowledge specifies the type of hallucination in the prompt application, and intricate reasoning (Zhao et al., when generating hallucinated text: 鈥淵ou are try- 2023). However, they invariably manifest halluci- ing to answer a question but misunderstand the nation (Rawte et al., 2023; Yu et al., 2024c), as they question context and intention鈥 (Li et al., 2023). often generate content that is incongruent with user Additionally, benchmarks such as HaDes annotate input, the model鈥檚 output context, or factual infor- hallucinations at a finer granularity by generating mation. Real-world hallucination examples from token-level hallucinations based on text perturba- our UHGEval dataset can be observed in Fig. 1. tions (Liu et al., 2022), but the text perturbation me
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：in undertaking scalable and reproducible ex- While there have been a bunch of efforts to de- periments. We have also evaluated prominent velop benchmarks for hallucination assessment, Chinese LLMs and the GPT series models to they always employ restricted techniques to pro- derive insights regarding hallucination. duce particular kinds of hallucinated utterances. This approach is at odds with real-world scenarios
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Comparisons with Other Datasets, Below are specific comparisons, Compared with the same, Below is a comparison
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - a detailed comparison with three other datasets, constructing datasets often yield biases towards
  - A Comparisons with Other Datasets 14
  - A Comparisons with Other Datasets placing them together for downstream models to
  - Below are specific comparisons with other datasets
  - Compared with the same period last year
  - Below is a comparison of the experimental out-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：5%, 80%, 3.2%, 200%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - tion are collected, and most benchmarks directly significantly enhances the pool of data available for
  - formation about significant news between 2015 and
  - complete news article into three parts: the begin- tent is indeed hallucinated presents a significant
  - lously balance examples in discriminative and also the sentence level, most LLMs exhibit improved
  - can be arrayed on a spectrum from simple to com- to collectively improve our dataset. Secondly, the
  - to solely improve the model鈥檚 capacity for reli-
  - other benchmark datasets to observe any significant

## 6. Conclusion、局限与可复现性

- **结论段落线索**：GPT4-1106 68.73% 60.19% 54.77% 62.04% InternLM-20B 51.88% 50.65% 49.56% 48.43% Each of the three evaluation forms possesses dis- Qwen-14B 62.81% 57.35% 53.15% 53.09% tinct advantages and drawbacks. Discriminative Xinyu-7B 48.44% 52.02% 50.87% 50.00% Xinyu2-70B 63.13% 61.47% 54.46% 57.07% evaluation is often the method of choice for a range of standard benchmarks (Li et al., 2023; Cheng Table 5: Evaluation by different types. In the same row, opti- et al., 2023). This approach is intuitive, and the mal values are bolded, and suboptimal values are underlined. construction of evaluation prompts is straightfor- ward. Selective evaluation resembles discrimina- tive evaluation but is marginally less demanding illustrated in Table 5. Initially, most LLMs demon- because it includes a reference option for contrast. strate enhanced accuracy for knowledge-intensive In both discriminative and selective evaluations, and document-intensive news. This may be be- certain models might be suspected of c
- **局限/未来工作线索**：limitations. For instance, this may involve em-；5 Conclusion deviations. We leave this for future work.；Limitations Lu, Keming Lu, Jianxin Ma, Rui Men, Xingzhang
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“UHGEval: Benchmarking the Hallucination of Chinese Large Language Models via Unconstrained Generation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
