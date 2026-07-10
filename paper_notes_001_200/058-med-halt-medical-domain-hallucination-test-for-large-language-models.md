# 058. Med-HALT: Medical Domain Hallucination Test for Large Language Models

> 逐篇阅读记录：第 58 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ankit Pal, Logesh Kumar Umapathi, Malaikannan Sankarasubbu
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.conll-1.21)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 11,631 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：This research paper focuses on the challenges posed by hallucinations in large language models (LLMs), particularly in the context of the medical domain.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Datasets Statistics；4 Data Analysis；300 NEET；250 TWMLE (Taiwan)；5 Experiments；6 Results；8 Conclusion Su, Yan Xu, Etsuko Ishii, Yejin Bang, Wenliang Dai,
- **引言关键线索**：of life or death. They pose real-life risks, as they Advancements in artificial intelligence, particu- could potentially affect healthcare decisions, diag- larly in the area of large language models (LLMs) nosis, and treatment plans. Hence, the development (Agrawal et al., 2022; Radford et al., 2019), have of methods to evaluate and mitigate such hallucina- led to transformative applications across various do- tions is not just of academic interest but of practical mains, including healthcare (Singhal et al., 2022). importance. These models possess the ability to understand and Efforts have been taken to mitigate the occur- generate human-like text, by learning patterns from rence of hallucinations in large language models vast corpora of text data. and making them valuable (Li et al., 2023a; Shuster et al., 2021; Liu et al., resources for medical professionals, researchers, 2021), but n
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：led to transformative applications across various do- tions is not just of academic interest but of practical mains, including healthcare (Singhal et al., 2022). importance. These models possess the ability to understand and Efforts have been taken to mitigate the occur- generate human-like text, by learning patterns from rence of hallucinations in large language models vast corpora of text data. and making them valuable (Li et al., 2023a; Shuster et al., 2021; Liu et al., resources for medical professionals, researchers, 2021), but not in the medical field. The purpose of and students. (Singhal et al., 2023; Han et al., 2023; this research work is to address the issue of halluci- Li et al., 2023b) Despite their impressive capabil- nation in large language models specifically within ities, they are also subject to unique challenges the medical domain. We propose a novel dataset 314 Proceedings of the 27th Conference on Computational Nat
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Comparison 1.3, De- 2019, Text-generation-inference library. The mod-, Baseline Models tio of, Comparison, These questions require compar-, Comparison Which of the, Multiforme
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - test measures the model鈥檚 capacity to identify Comparison 1.3%
  - comparison, and natural language inference. De- 2019) Text-generation-inference library. The mod-
  - 5.1 Baseline Models tio of the correct predictions to the total predictions
  - Comparison: These questions require compar- calculate the rate at which models deviated from
  - Comparison Which of the following is most malignant tumor? 鈥0鈥: 鈥橤lioblastoma Multiforme鈥, 鈥1鈥: 鈥橫enin-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 point, 0.25 points, 1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - con, revealing significant differences in their
  - dataset significantly enhances the diversity of Med- question where the correct answer is replaced
  - and LlaMa-2 outperform commercial variants such
  - Llama-2 70B outperformed other models with Instruction tuned (Wei et al., 2021; Bai et al., 2022;
  - an accuracy of 42.21% and a score of 52.37 in the Wang et al., 2022) models have shown to improve
  - struct) outperformed OpenAI鈥檚 GPT-3.5 and Text-
  - substantial room for improvement across all mod- take GPT-3.5 and measure the performance across

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Andrea Madotto, and Pascale Fung. 2022. Survey of This research advances our understanding of hallu- hallucination in natural language generation. ACM cination in large language models (LLMs) within Computing Surveys, 55:1 鈥 38. the medical domain, introducing the Med-HALT dataset and benchmark as a comprehensive tool for Di Jin, Eileen Pan, Nassim Oufattole, Wei-Hung Weng, Hanyi Fang, and Peter Szolovits. 2020. What dis- evaluating and mitigating such issues. Our com- ease does this patient have? a large-scale open do- parative analysis of models, including OpenAI鈥檚 main question answering dataset from medical exams. Text-Davinci, GPT-3.5, Llama-2, and Falcon, has ArXiv, abs/2009.13081. revealed considerable room for improvement. Junyi Li, Xiaoxue Cheng, Wayne Xin Zhao, Jianyun Nie, and Ji rong Wen. 2023a. Halueval: A large- scale hallucination evaluation benchmark for large References language models. ArXiv, abs/2305.11747. Monica Agrawal, Stefan Hegselmann, Hunter Lang, Yoon Kim, an
- **局限/未来工作线索**：Limitations & Future Scope Max D tokens 37 661；Our study has a few limitations and also presents
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Med-HALT: Medical Domain Hallucination Test for Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
