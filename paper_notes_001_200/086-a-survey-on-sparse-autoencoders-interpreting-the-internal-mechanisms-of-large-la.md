# 086. A Survey on Sparse Autoencoders: Interpreting the Internal Mechanisms of Large Language Models

> 逐篇阅读记录：第 86 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Dong Shu, Xuansheng Wu, Haiyan Zhao, Daking Rai, Ziyu Yao, Ninghao Liu, Mengnan Du
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-emnlp.89)
- **领域标签**：SAE, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 15,481 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型内部特征叠加、神经元语义不纯导致难以解释的问题。。方法上以SAE为主线，通过表征分析、因果干预或解释评估来连接内部机制与外部行为。
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction approach has shown promise in transforming the；4 Evaluation Metrics and Methods；5 Applications in Large Language Models SAEs construct a dictionary of concepts through；2024. Decoding dark matter: Specialized sparse Dowling, Sheila Dunning, Adrien Ecoffet, Atty Eleti,
- **引言关键线索**：often-inscrutable activations of LLMs into more Large Language Models (LLMs), such as GPT- human-understandable representations, potentially 4 (OpenAI et al., 2024), Claude-3.5 (Anthropic, creating a more effective vocabulary for mechanis- 2024), DeepSeek-R1 (DeepSeek-AI et al., 2025), tic analysis of these complex systems. and Grok-3 (xAI, 2025), have emerged as powerful tools in natural language processing, demonstrating 1.1 Contribution and Uniqueness remarkable capabilities in tasks ranging from text Our Contributions. In this paper, we provide a generation to complex reasoning. However, their comprehensive overview of SAE for LLM inter- increasing size and complexity have created signif- pretability, with major contributions listed as fol- icant challenges in understanding their internal rep- lowing: (1) We explore the technical framework resentations and decision-making processes. 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tracted significant attention from the research LLMs, Sparse Autoencoders (SAEs) (Cunningham community as a means to understand the inner et al., 2023; Bricken et al., 2023; Gao et al., 2025; workings of LLMs. Among various mechanis- Rajamanoharan et al., 2024b; Galichin et al., 2025) tic interpretability approaches, Sparse Autoen- have emerged as a particularly promising direction coders (SAEs) have emerged as a promising for addressing a fundamental challenge in LLM method due to their ability to disentangle the interpretability: polysemanticity. Many neurons in complex, superimposed features within LLMs LLMs are polysemantic, responding to seemingly into more interpretable components. This pa- per presents a comprehensive survey of SAEs unrelated concepts or features simultaneously. This for interpreting and understanding the internal is a phenomenon likely resulting from superposi- workings of LLMs. Our major contributions tion (Elh
- **方法与解释性关系**：该论文主要围绕 `SAE, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Gur-Arieh et al, Compared with VocabProj, Feature Geometry detailed comparison, SAEs and probing-, Steering llms, Therefore, The standard We compare, SAE per layer, SAE activations against a, Evaluation and Comparison of, SAEs dictive behavior more, The explained variance also, SAE latents racies that, LLM baseline
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - 2025b; Gur-Arieh et al., 2025) find that output- Compared with VocabProj, that only considers
  - or overlap with other neurons. Feature Geometry detailed comparison between SAEs and probing-
  - Steering llms? even simple baselines outperform
  - ings not only deepen our understanding of model looking well-known baselines. Therefore, it is im-
  - for different layers of the model. The standard We compare the performance of sparse probes us-
  - approach is to train one SAE per layer, meaning ing SAE activations against a baseline probe using

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X, 8x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - tracted significant attention from the research LLMs, Sparse Autoencoders (SAEs) (Cunningham
  - improvements, and effective training strategies; of neurons. SAEs address this issue by learning
  - 鈥渂lack box鈥 nature of LLMs has sparked a grow- ous design improvements, and effective training
  - Improve Architecture (搂C.1)
  - Improve Training Strategy (搂C.2)
  - of traditional SAEs, each introducing improve- us a clue to understanding the semantic meaning
  - groups: Improve Architectural, which modify the spans that maximally activate a feature vector,

## 6. Conclusion、局限与可复现性

- **结论段落线索**：els. One study focuses on general ICL tasks, and In this survey, we provided a comprehensive exam- task-related function vectors were successfully iso- ination of SAEs as a promising approach to inter- lated (Kharlapenko et al., 2024). Another study fo- preting and understanding LLMs. SAEs effectively cuses on reinforcement learning (RL) tasks. Their address the challenge of polysemanticity through experiment shows that an LLM鈥檚 internal represen- learning overcomplete, sparse representations that tations are capable of capturing temporal difference disentangle superimposed features into more inter- errors and Q-values that are essential in RL com- pretable units. We have systematically explored putations (Demircan et al., 2025). Besides, one the foundational principles, technical frameworks, study attempts to examine the working mechanism evaluation methodologies, and real-world appli- of instruction following. Their analysis on trans- cations of SAEs in the context of LLM analysis. l
- **局限/未来工作线索**：ants have been proposed to address the limitations summarizing the most activated text spans can give；Due to page limitations, examples for each group the minimal context necessary to preserve strong；Limitations Conference on Machine Learning, pages 2397鈥2430.；Due to page limitations, we do not attempt to pro- In late 2023, Anthropic advanced transformer in-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“A Survey on Sparse Autoencoders: Interpreting the Internal Mechanisms of Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
