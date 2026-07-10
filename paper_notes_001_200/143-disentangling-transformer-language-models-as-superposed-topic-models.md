# 143. Disentangling Transformer Language Models as Superposed Topic Models

> 逐篇阅读记录：第 143 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jia Lim, Hady W. Lauw
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.534)
- **领域标签**：SAE, Hidden, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 16,062 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型内部特征叠加、神经元语义不纯导致难以解释的问题。。方法上以SAE为主线，结合论文摘要中的核心设定：Topic Modelling is an established research area where the quality of a given topic is measured using coherence metrics.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction tionally feasible approach to solve it via heuristics；2 Related Work 2-XL with three observable areas: 1) Elimination at the；8 Extending to LLaMA ences are significant, their values are similar or；9 Discussions come when pursuing prompt-less explanations of
- **引言关键线索**：The term 鈥榮uperposition鈥 in machine learning typi- and exact solving (see Section 5). Wary of mirages cally refers to overlapping concepts. Elhage et al. (Schaeffer et al., 2023), we propose a non-trivial (2021) allude to the property of superposition in baseline and empirically show that the disentangled transformers when discussing the difficulty of inter- superposition interpretations are indeed meaningful preting its multilayer perceptrons鈥 (MLP) neurons and not due to chance (see Section 6). in Transformer Language Models (TLM). Black Contributions. This work aims to deploy topic et al. (2022) also find neurons in transformers hav- modelling methodologies, specifically in the area ing polysemantic behaviour. We believe it is possi- of interpretability, to automate the search and eval- ble to interpret neurons in superposition, motivated uation of concepts within TLMs. We propose a b
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tangle decoder-only TLM, potentially mapping human notion of coherence (Mimno et al., 2011; individual neurons to multiple coherent topics. Lau et al., 2014; R枚der et al., 2015), and still de- Our results show that it is empirically feasi- bated (Doogan and Buntine, 2021; Hoyle et al., ble to disentangle coherent topics from GPT-2 2021). We show that their findings might help ad- models using the Wikipedia corpus. We val- vance the interpretation of TLM (see Section 3.1). idate this approach for GPT-2 models using Our third insight contributes towards tackling the Zero-Shot Topic Modelling. Finally, we ex- tend the proposed approach to disentangle and disentanglement problem from a novel angle. We analyse LLaMA models. map the disentanglement problem to an NP-Hard classical graph problem and propose a computa-
- **方法与解释性关系**：该论文主要围绕 `SAE, Hidden, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Baseline, We use a random, As G has a, The random baseline uses, The random baseline pro-, GPT-2 and random baseline, Word Sets, Word Pool are significant, NPMIW scores between GPT-2-sampled, Topical Pools and, In this ZSTM task, Table 4, The best baseline scores
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - (2021) allude to the property of superposition in baseline and empirically show that the disentangled
  - allows parity in comparison across differently-sized
  - Baseline. We use a random distribution of log-
  - 1972). As G has a non-trivial amount of vertices, terpretability. The random baseline uses the same
  - tins (2015) explored the feasibility of solving differ- 2 to its random baseline, differences in word
  - and vertex i by boolean variables ei,j (Eq. 13f) and sets remains similar. The random baseline pro-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Our results show that it is empirically feasi- bated (Doogan and Buntine, 2021; Hoyle et al.,
  - ble to disentangle coherent topics from GPT-2 2021). We show that their findings might help ad-
  - themes. To further validate our results dynamically,
  - validate our approach in Section 6, where we show
  - ent convex formulations of MEKC on G of varying sets/neuron are significant (see Table 1). We ob-
  - selected clique (Eq. 13f, 13g). sampling a significantly larger number of words.
  - differences in distributions between GPT-2 and random baseline on number of Word Sets/Word Pool are significant

## 6. Conclusion、局限与可复现性

- **结论段落线索**：TLMs. Aside from comparing across different sizes within the model family, we can also compare across 10 Conclusion model families. There are differences between the We demonstrate that our approach from a novel an- results between GPT-2 and LLaMA. In this section, gle, disentangling superposed topics from TLM via we discuss the possible cause and effect. a graph-based formulation optimizing topic mod- Topical Sparsity. The larger sizes of TLMs elling metric NPMIW , revealing and analysing su- might induce greater topical sparsity, with larger perposed topics on GPT-2, evaluated on ZSTM, models able to spread their learnt concepts over a and extendable to LLaMA. Additionally, we show larger number of neurons. With greater topical spar- that TLM is quantifiable by topic coherence and sity, there is greater difficulty in inferring concepts the number of superposed topics in an automated directly from the neuron as its characteristics may manner. Comparison using a random baseline gives b
- **局限/未来工作线索**：Limitations David M. Blei, Andrew Y. Ng, and Michael I. Jordan.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Disentangling Transformer Language Models as Superposed Topic Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
