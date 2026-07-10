# 138. Explainable Claim Verification via Knowledge-Grounded Reasoning with Large Language Models

> 逐篇阅读记录：第 138 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Haoran Wang, Kai Shu
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.416)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 11,739 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Claim verification plays a crucial role in combating misinformation.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction early promising results, they rely on the availabil-；3 Method；4 Experiments；6 Limitations ings expressed are those of the author and should；2023. Predicting information pathways across online；2022. Selection-inference: Exploiting large language
- **引言关键线索**：Claim verification (Guo et al., 2022) has become ity of large-scale human-annotated datasets, which increasingly important due to widespread online pose challenges due to labor-intensive annotation misinformation (Tian et al., 2023; Jin et al., 2023). efforts and the need for annotators with specialized Most of the existing claim verification models domain knowledge. To address the issue of creat- (Zhou et al., 2019; Jin et al., 2022; Yang et al., ing large-scale datasets, recent works (Pan et al., 2022; Wadden et al., 2022b; Liu et al., 2020; Zhong 2021; Wright et al., 2022; Lee et al., 2021) focus on et al., 2020) use an automated pipeline that consists claim verification in zero-shot and few-shot scenar- ios. However, these methods follow the traditional 1 https://github.com/wang2226/FOLK claim verification pipeline, requiring both claim 6288 Findings of the Association for Computatio
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：1 https://github.com/wang2226/FOLK claim verification pipeline, requiring both claim 6288 Findings of the Association for Computational Linguistics: EMNLP 2023, pages 6288鈥6304 December 6-10, 2023 漏2023 Association for Computational Linguistics and annotated evidence for veracity prediction. Ad- experiment results demonstrate that FOLK can ver- ditionally, these models often lack proper justifi- ify complex claims while generating explanations cations for their predictions, which are important to justify its decision-making process. Additionally, for human fact-checkers to make the final verdicts. we show the effectiveness of FOL-guided claim de- Therefore, we ask the following question: Can we composition and knowledge-grounded reasoning develop a model capable of performing claim verifi- for claim verification. cation without relying on annotated evidence, while In summary, our contributions are: generating natural language justificat
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：LLMs to generate a, We compare our proposed, We evaluate the explanation, LLMs with manual evaluation, Further- Direct This baseline, LLM as, We compare FOLK to, The baselines and FOLK, GPT-3, CoT and Self-Ask baselines, Table 2. plex
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - performs strong baselines on three datasets label and generate explanation.
  - LLMs to generate a paragraph of human-readable We compare our proposed method against the fol-
  - explanation. We evaluate the explanation gener- lowing four baselines.
  - ated by LLMs with manual evaluation. Further- Direct This baseline simulates using LLM as
  - We compare FOLK to existing methods on 7 claim lar approach that demonstrates chains of inference
  - set based on the number of hops: two-hop claims, posed baseline for verifying complex claims using

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：5.8x, 8x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - for human fact-checkers to make the final verdicts. we show the effectiveness of FOL-guided claim de-
  - ery et al., 2022). Figure 1 illustrates a real-world 鈥 We show that FOLK can generate high-quality
  - demonstrates effectiveness on significantly smaller Although LLMs have displayed decent perfor-
  - augment with LLMs to improve the factuality of language models (Valmeekam et al., 2022; Elazar
  - ified sampling to select 100 examples from each main question. It is shown to improve the perfor-
  - 4.3 Experiment Settings On FEVEROUS dataset, FOLK outperforms the
  - prompts used varies between 4-6 between the proach outperforms CoT and Self-Ask baselines on

## 6. Conclusion、局限与可复现性

- **结论段落线索**：interest and significantly reduce the workload of In this paper, we propose a novel approach to human fact-checkers. However, it is essential to rec- tackle two major challenges in verifying real-world ognize that they may also be susceptible to misuse claims: the scarcity of annotated datasets and the by malicious individuals. Therefore, we strongly absence of explanations. We introduce FOLK, a urge researchers to approach their utilization with reasoning method that leverages First-Order Logic caution and prudence. to guide LLMs in decomposing complex claims Environmental Impact. We want to highlight into sub-claims that can be easily verified through the environmental impact of using large language knowledge-grounded reasoning with LLMs. models, which demand substantial computational Our experiment results show that FOLK demon- costs and rely on GPUs/TPUs for training, which strates promising performance on three challenging contributes to global warming. However, it is worth datase
- **局限/未来工作线索**：Contemporaneous to our work, (Peng et al., reasoning problems. This limitation arises from；6 Limitations ings expressed are those of the author and should；We identify two main limitations of FOLK. First, or policies of the Department of Defense or the U.S.；tion for future work. Second, FOLK has a much tured and structured information. arXiv preprint
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Explainable Claim Verification via Knowledge-Grounded Reasoning with Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
