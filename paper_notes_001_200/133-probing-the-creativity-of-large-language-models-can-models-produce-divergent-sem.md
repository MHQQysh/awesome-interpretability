# 133. Probing the “Creativity” of Large Language Models: Can models produce divergent semantic association?

> 逐篇阅读记录：第 133 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Honghua Chen, Nai Ding
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.858)
- **领域标签**：Probing, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 4,693 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Large language models possess remarkable capacity for processing language, but it remains unclear whether these models can further generate creative content.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Measuring Creativity Formally, given n words and their word embed-；3 Experiment Setup we also compute the average surprisal (negative log；4 Result；489. Number: 7587 Publisher: Nature Publishing；2023. Can chatgpt be used to generate scientific；2023. Retrieval flexibility links to creativity: evi-
- **引言关键线索**：recursion for LLMs that training on generated data Large language models (LLMs) have exhibited un- makes models collapse, one promising solution paralleled mastery of natural language (Bubeck might be the novel language distribution through et al., 2023). The primary capacity of producing the creative generation (Shumailov et al., 2023). But most probable next word is broadly generalizable since LLMs represent word meaning and predict to many language tasks, suggesting underlying cog- the next word in context, it seems paradoxical that nitive abilities beyond specialized linguistic rules such models could create ideas not seen in training. and patterns. There is observation that LLMs may Here, we empirically investigate the creativity of possess reasoning abilities which is a core aspect of LLMs by examining models鈥 ability to generate intelligence, including decision-making (Binz and di
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：et al., 2020). These texts have human-like informa- general cognitive process of creativity.2 Divergent tion distribution, which are viewed as informative thinking, i.e., generating a large variety of solu- and interesting (Meister et al., 2022). Despite this, tions from an open-ended point, is an indicator stochastic strategies are distinct from human. The for creative thinking (Beaty et al., 2014). One of decoding process is probabilistic and independent, the most widely used tasks on divergent thinking while humans produce language under elaborated is the alternate use task (AUT), which asks par- cognitive control, especially for creative generation. ticipants to generate unusual uses of objects (e.g., Creative and nonsense contents are both infrequent a brick) (Guilford, 1964). Previous studies used during next word prediction that cannot be simply AUT to measure the creativity of LLMs (Summers- distinguished via sampling strategies
- **方法与解释性关系**：该论文主要围绕 `Probing, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We compare the results, RLHF, Ouyang prompts for comparison, Base, Thus, DAT with troversial that, Figure 10, The results are similar, We find positive are, However, GPT-4, DAT scores partially depend, DAT, Contours indicate the 95
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - them. We compare the results across differ-
  - performance through pre-train and RLHF (Ouyang prompts for comparison: (1) Base: write 10 nouns,
  - relevant for human and random baselines (also
  - invalid as well. Thus, we compare the DAT with troversial that requires evaluations from multiple
  - human and random and baselines surprisal (Figure 10 ). The results are similar as
  - on human and random baselines. We find positive are below average human level. However, GPT-4

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：100 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - search strategy, GPT-4 outperforms 96% of
  - tic distance. Indeed, we find two variables highly
  - The DAT and surprisal for more LLMs are shown in Turbo outperform average human performance, but
  - 1985; Fan et al., 2018). We vary the tempera- 5 Conclusion
  - decoding strategies. We find GPT-4 demonstrates
  - prisal, we find a naive algorithm that samples nouns
  - from Wordnet can outperform the majority of hu-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ture from 0.1 to 1 and found its positive effect In this work, we provide a creativity evaluation for models except GPT-4. However, the effect is using a divergent semantic task. This task reveals limited, and high temperature will also bring insta- distinguishable results across various LLMs and bility and produce invalid answers. As for GPT-4, decoding strategies. We find GPT-4 demonstrates high-probability tokens are well aligned with high- advanced human-level creativity stably, while GPT- quality answers. 3.5-turbo exceeds the average human level. For In the relationship between the DAT and sur- decoding methods, stochastic decoding strategy is prisal, we find a naive algorithm that samples nouns effective but not enough for creative generation. from Wordnet can outperform the majority of hu- mans and models (Figure 4). It is because the Limitation algorithm has no constrains of the language distri- bution, which also means it can barely accomplish Creativity is a deeply debated c
- **局限/未来工作线索**：mans and models (Figure 4). It is because the Limitation
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Probing the “Creativity” of Large Language Models: Can models produce divergent semantic association?”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
