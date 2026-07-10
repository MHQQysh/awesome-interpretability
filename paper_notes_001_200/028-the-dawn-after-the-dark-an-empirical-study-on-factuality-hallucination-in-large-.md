# 028. The Dawn After the Dark: An Empirical Study on Factuality Hallucination in Large Language Models

> 逐篇阅读记录：第 28 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Junyi Li, Jie Chen, Ruiyang Ren, Xiaoxue Cheng, Xin Zhao, Jian‐Yun Nie, Ji-Rong Wen
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.586)
- **领域标签**：Analysis, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 14,348 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Junyi Li, Jie Chen, Ruiyang Ren, Xiaoxue Cheng, Xin Zhao, Jian-Yun Nie, Ji-Rong Wen.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction ment learning from human feedback (RLHF), and；2 Hallucination in LLMs to generate hallucinations across various domains,；4 Hallucination Detection man labelers to annotate the hallucination type of；60 Finance Education；6 Hallucination Mitigation biomedicine and open domain, while showing mild；7 Related Work mitigation during generation is mainly focused on；9 Limitations Michael Pieler, USVSN Sai Prashanth, Shivanshu；2023. Falcon-40B: an open large language model
- **引言关键线索**：inference, and thus can conduct more in-depth anal- Large language models (LLMs) (Zhao et al., 2023) ysis of potential impact of each stage on model hal- have shown remarkable potential in a wide range of lucination. This analysis approach is quite different natural language processing applications (Brown from prior work, where they mostly study the im- et al., 2020; Ouyang et al., 2022; OpenAI, 2023). pact of individual stages or strategies to attribute or However, despite the significant improvement in mitigate the hallucinations. model capacity, a persistent challenge lies in their To conduct our empirical study, we first extend tendency to hallucinate, i.e., generate content that previous work (Li et al., 2023a) and construct a new looks plausible but is factually incorrect (Huang benchmark HaluEval 2.0 for evaluating the factu- et al., 2023; Ji et al., 2023a; Zhang et al., 2023b). a
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：LLM hallucinations. Furthermore, we zoom research the aforementioned three questions. into the different training or utilization stages of LLMs and extensively analyze the poten- For deciphering the mystery of hallucination in tial factors that lead to the LLM hallucina- LLMs, we aim to conduct a comprehensive and sys- tions. Finally, we implement and examine tematic empirical study on hallucination detection, a series of widely used techniques to miti- source, and mitigation. In particular, we mainly gate the hallucinations in LLMs. Our work focus on studying factuality hallucination, which has led to several important findings to under- has become one of the primary erroneous sources stand the hallucination origin and mitigate the for LLMs (Huang et al., 2023; Zhang et al., 2023b). hallucinations in LLMs. Our code and data can be accessed at https://github.com/ To carry out our research, we zoom into the dif- RUCAIBox/HaluEval-2.0. fe
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：For a fair comparison, In this part, Section C.2 for comparison
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - and then describe a set of models for comparison.
  - amine combinatorial effects. For a fair comparison In this part, we will study the effects of decoding
  - performance comparison. lar role (e.g., scientist) for the system in the base
  - Section C.2 for comparison. prompt engineering, it is crucial to comprehen-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - In the era of LLMs, there has been a significant
  - However, despite the significant improvement in mitigate the hallucinations.
  - LLMs significantly influences the source of halluci- Note that it is difficult to encompass all kinds of
  - with improved and appropriate instructions can be We present several illustrative examples for each
  - than a threshold. In our benchmark, questions in approach can significantly reduce the costs of data
  - 1 X Count(hallucinatory f acts) Table 1 might significantly exceed the actual rate in
  - significant performance gap between open-source

## 6. Conclusion、局限与可复现性

- **结论段落线索**：sekgonul et al., 2023; Azaria and Mitchell, 2023a). Typically, the internal states that can be studied This paper presented a comprehensive empirical by examining the output logit values, the hidden analysis about LLM hallucinations in the three as- layer activations, and the attention states. For ex- pects of detection, source and mitigation. We con- ample, Varshney et al. (2023) leveraged the output structed the hallucination benchmark HaluEval 2.0 logit values of the model as a signal of halluci- and developed an LLM-based automatic detection nations to estimate the uncertainty of responses. approach. Based on this benchmark, we further For models that can only be accessed through API systematically investigated the possible sources for calls, hallucinations are typically studied by ana- LLM hallucination in the stages of pre-training, lyzing the relationship between input prompts and SFT, RLHF, and inference, and also examined the the model鈥檚 output responses (Rawte et al., 2023b; 
- **局限/未来工作线索**：eral data (e.g., webpages) and specialized data (e.g., cussion of data cleaning in future work.；mitigating hallucinations in future work. the applications of SFT (Wang et al., 2022b) and；9 Limitations Michael Pieler, USVSN Sai Prashanth, Shivanshu；tailed analysis as future work. Furthermore, our ing to rank dataset for medical information retrieval.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“The Dawn After the Dark: An Empirical Study on Factuality Hallucination in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
