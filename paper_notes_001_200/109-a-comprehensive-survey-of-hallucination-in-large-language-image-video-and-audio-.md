# 109. A Comprehensive Survey of Hallucination in Large Language, Image, Video and Audio Foundation Models

> 逐篇阅读记录：第 109 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Pranab Sahoo, Prabhash Meharia, Akash Ghosh, Sriparna Saha, Vinija Jain, Aman Chadha
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-emnlp.685)
- **领域标签**：Analysis, MLLM, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 10,138 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Analysis为主线，通过表征分析、因果干预或解释评估来连接内部机制与外部行为。
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction by a model across different modalities is inconsis-；2 Hallucination in Large Language；4 Hallucinations in Large Video Models a streaming model comprising a memory module；5 Hallucinations in Large Audio Models nuanced and intricate nature of emotions conveyed；6 Future Directions；7 Conclusion Neelakantan, Pranav Shyam, Girish Sastry, Amanda；I Chern, Steffi Chern, Shiqi Chen, Weizhe Yuan, Kehua Chen, Arun Tejasvi Chaganty, Yicheng Fan, Vin-；2023. Generating benchmarks for factuality verse drug event detection with multimodal dataset:
- **引言关键线索**：The rapid progress in large-scale foundation mod- tent or out of sync with the context that the user els (FMs), spanning language, image, audio, and or the input data provided or expected. Semantic video domains, has revolutionized the field of ar- distortion: Tjio et al. (2022) refers to a type of tificial intelligence (AI). Models such as GPT- inconsistency or error in generated content where 3 (Brown et al., 2020), MiniGPT-4 (Zhu et al., the semantics or underlying meaning of the input 2023), AudioLLM (Borsos et al., 2023), and LaV- is misrepresented or altered in the output. Con- iLa (Zhao et al., 2022) have demonstrated remark- tent hallucination is the term used to describe a able abilities across diverse tasks, from text genera- phenomenon seen in generative models when fea- tion to multimodal understanding. As these models tures or elements that are generated as output are find w
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：nition, taxonomy, and detection strategies for 1.2 Types of Hallucination addressing hallucination in multimodal founda- Hallucinations in large FMs can manifest in various tion models, laying the foundation for future forms and can be categorized as follows: Contex- research and development in this pivotal area. tual disconnection: Zhang et al. (2023d) described a situation in which the output or content produced
- **方法与解释性关系**：该论文主要围绕 `Analysis, MLLM, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：VL models in discerning, GPT4-Vision, Nishimura et al, This survey paper aims
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - ates the performance of 10 VL models in discerning assessment and comparison to GPT4-Vision, with
  - of tasks, including retrieval, generation, and clas- pervised baseline models. Nishimura et al. (2024)
  - soning abilities compared to baseline models on video modalities. This survey paper aims to provide

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - overview of recent developments that aim to mation, incorrect conclusions, and adverse effects
  - sures their reliability. Huang and Chang (2023) evidence, as a significant gap.
  - (2024c) introduced the Sorry, Come Again (SCA) evidence chains to improve assessment accuracy.
  - pact on the hallucinated generation, emphasizing garnered significant attention in the AI commu-
  - improved stability and flexibility in assessing ob- posed ChartBench, a benchmark assessing chart
  - orientations and introduced a dataset of 2,000 sam- called FactVC, which outperforms previous met-
  - ing language-only data limitations. models by a significant margin. To enhance dense

## 6. Conclusion、局限与可复现性

- **结论段落线索**：identify and mitigate the problem of hallucina- in critical applications. Mitigating hallucinations tion in FMs, spanning text, image, video, and has become an active research focus, with strategies audio modalities. By synthesizing recent ad- like fine-tuning with domain-specific data, using vancements in detecting and mitigating halluci- diverse, robust training data, and developing im- nation across various modalities, the paper aims to provide valuable insights for researchers, de- proved evaluation metrics to identify and reduce velopers, and practitioners. Essentially, it es- hallucination tendencies. tablishes a clear framework encompassing defi- nition, taxonomy, and detection strategies for 1.2 Types of Hallucination addressing hallucination in multimodal founda- Hallucinations in large FMs can manifest in various tion models, laying the foundation for future forms and can be categorized as follows: Contex- research and development in this pivotal area. tual disconnection: Zha
- **局限/未来工作线索**：ing language-only data limitations. models by a significant margin. To enhance dense；et al. (2023) introduced CompA, consisting of 8 Limitation
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“A Comprehensive Survey of Hallucination in Large Language, Image, Video and Audio Foundation Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
