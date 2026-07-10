# 149. Theory of Mind for Multi-Agent Collaboration via Large Language Models

> 逐篇阅读记录：第 149 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Huao Li, Chong Yu, Simon Stepputtis, Joseph Campbell, Dana Hughes, Charles Lewis, Katia Sycara
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.13)
- **领域标签**：Representation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 8,641 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：While Large Language Models (LLMs) have demonstrated impressive accomplishments in both reasoning and planning, their abilities in multi-agent collaborations remains largely unexplored.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction tions with humans, their social intelligence is ex-；2 Related Work；5 Experiments；7 Discussions able work ahead for LLMs to develop a functional；9 Acknowledgements；2022. Lamda: Language models for dialog applica-
- **引言关键线索**：Recent large language models (LLMs), such as pected to improve for them to become effective col- GPT-4 (OpenAI, 2023), have demonstrated im- laborators (Williams et al., 2022; Li et al., 2022). pressive competencies across a wide array of do- For instance, a proficient AI assistant should be mains and tasks, ranging from mathematics to law, able to infer a human鈥檚 preferences based on pre- without the need for fine-tuning or special prompt- vious experiences without needing to ask. Recent ing (Bubeck et al., 2023). This advancement has studies have applied classic Theory-of-Mind tasks significantly transformed the landscape of Natu- to several LLMs, concluding that current mod- ral Language Processing (NLP) research. Instead els (e.g., GPT-4) perform comparably to 9-year- of developing domain-specific models for down- old children (Kosinski, 2023). However, the re- stream applications, f
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：bounds of computer science and integrates insights ios, encompassing dynamic belief state evolution from diverse scientific fields (Rahwan et al., 2019). and rich intent communication between multiple Drawing inspiration from team science and group agents. psychology (Hagendorff, 2023), our study concen- The main contributions of this study include that trates on collective machine behavior, evaluating we: 180 Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing, pages 180鈥192 December 6-10, 2023 漏2023 Association for Computational Linguistics 鈥 Evaluate LLM-based agents鈥 embodied in- 2023; Moghaddam and Honey, 2023). Results indi- teraction capability in multi-agent collabora- cate that leading LLMs can pass more than 90% of tive tasks against reinforcement learning and these test cases. In contrast, Ullman (2023) found planning-based baselines that LLMs struggle with complex ToM inferences involving c
- **方法与解释性关系**：该论文主要围绕 `Representation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We observed evi- et, Motivated to explore this, LLMs struggle with complex, ToM inferences, Table 1, Task performance of LLM-based, Score represent the average, Baselines precedence and temporal, MARL and plan- sum, Specifically, However, LLM-based agents, ChatGPT fails to complete, We identify, Additionally, The second type of, Percentages represent the
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - planning-based baselines. We observed evi- et al., 2023). Motivated to explore this argument,
  - planning-based baselines that LLMs struggle with complex ToM inferences
  - Table 1: Task performance of LLM-based agents and baseline conditions. Score represent the average team score in
  - 5.2 Baselines precedence and temporal constraints in order to
  - also include baselines based on MARL and plan- sum of path costs or makespan. Specifically, the
  - baseline given its centralized coordination and per- from massive language materials, acquire essential

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：10 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - LLM-based agents. Our results reveal limi- ing unknown environments, maintaining beliefs
  - Recent large language models (LLMs), such as pected to improve for them to become effective col-
  - significantly transformed the landscape of Natu- to several LLMs, concluding that current mod-
  - over, there is a significant body of research focusing
  - ance to improve sample efficiency and memory are then partitioned using a hyperparameter, the
  - corporation of belief state representation improves sess. Even though the information about room
  - other members. Other collaborative behaviors com- their belief state in text. Results show that the intro-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ToM and interact naturally with humans. Our study Our study yields three primary insights. First, represents a preliminary effort to devise novel eval- Large Language Models (LLMs) demonstrate sub- uation methods for LLMs鈥 ToM that go beyond stantial planning and collaboration capabilities traditional tests such as the Sally-Anne test. within our task scenarios. With suitable prompt- engineering, teams of LLM-based agents perform 8 Conclusions comparably to state-of-the-art Multi-Agent Rein- forcement Learning (MARL) algorithms. This find- In this study, we assessed the ability of recent large ing is particularly noteworthy given that MARL language models (LLMs) to conduct embodied agents receive extensive task-specific training with interactions in a team task. Our results demon- a centralized critic, while LLM-based agents oper- strate that LLM-based agents can handle complex ate in a fully decentralized manner and undertake multi-agent collaborative tasks at a level compara- tasks i
- **局限/未来工作线索**：Due to the model input size limitation, LLM-based agents and evaluate them in a collaborative task；highlighting LLMs鈥 limitations in generating ac- algorithm. We also observed evidence of emer-；this study also has its limitations. Currently, human Eean R Crawford and Jeffery A Lepine. 2013. A con-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Theory of Mind for Multi-Agent Collaboration via Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
