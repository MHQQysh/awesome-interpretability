# 121. Interpretable Preferences via Multi-Objective Reward Modeling and Mixture-of-Experts

> 逐篇阅读记录：第 121 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Haoxiang Wang, Wei Xiong, Tengyang Xie, Han Zhao, Tong Zhang
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-emnlp.620)
- **领域标签**：Evaluation, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 7,140 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Reinforcement learning from human feedback (RLHF) has emerged as the primary method for aligning large language models (LLMs) with human preferences.The RLHF process typically starts by training a reward model (RM) using
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：4 Experiment；5 Conclusion；2. Evaluation benchmark: We evaluated our ap- datasets, and evaluation benchmarks continue to；3. Language and domain coverage: Our exper-；4. Potential risks: While our approach aims to；2022. Training a helpful and harmless assistant with Askell, Yuntao Bai, Saurav Kadavath, Ben Mann,；345. Josh Jacobson, Scott Johnston, Shauna Kravec,
- **引言关键线索**：Reinforcement learning from human feedback In this paper, we explore the role of reward mod- (RLHF) has emerged as the primary method for els (RMs) within the framework of Reinforcement aligning large language models (LLMs) with Learning from Human Feedback (RLHF). RMs human preferences. The RLHF process typi- play a crucial role in aligning large language mod- cally starts by training a reward model (RM) us- els (LLMs) as they provide a scalable way to inte- ing human preference data. Conventional RMs grate human preferences into the models鈥 training are trained on pairwise responses to the same process, guiding the optimization of their policies. user request, with relative ratings indicating which response humans prefer. The trained RM To be more specific and provide more context, we serves as a proxy for human preferences. How- first review the most standard and popular RLHF ever, du
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：aligning large language models (LLMs) with Learning from Human Feedback (RLHF). RMs human preferences. The RLHF process typi- play a crucial role in aligning large language mod- cally starts by training a reward model (RM) us- els (LLMs) as they provide a scalable way to inte- ing human preference data. Conventional RMs grate human preferences into the models鈥 training are trained on pairwise responses to the same process, guiding the optimization of their policies. user request, with relative ratings indicating which response humans prefer. The trained RM To be more specific and provide more context, we serves as a proxy for human preferences. How- first review the most standard and popular RLHF ever, due to the black-box nature of RMs, their frameworks and the role of RMs in this frame- outputs lack interpretability, as humans cannot work. Arguably the dominant RLHF approach is a intuitively understand why an RM thinks a re- deep rein
- **方法与解释性关系**：该论文主要围绕 `Evaluation, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We believe that this, AI algorithm is a, To summarize, RLHF rized into pairwise, Overall, Table 1, Performance comparison on RewardBench, The benchmark consists of, Biometrika, Tom Henighan, Danny Hernandez, Tristan Hume, In arXiv
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - man preferences. We believe that this human-AI algorithm is a strong baseline, especially in rea-
  - To summarize, all the existing popular RLHF rized into pairwise comparisons, using the Overall
  - Table 1: Performance comparison on RewardBench. The benchmark consists of four primary categories (weight
  - pairwise comparisons of test examples. A straight- where the penalty coefficient 位i is chosen such
  - of paired comparisons. Biometrika, 39(3/4):324鈥 Tom Henighan, Danny Hernandez, Tristan Hume,
  - with pairwise comparison and generative fusion. In arXiv:2312.00886.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - to evaluate reward models for language model- gating weights significantly outperform the fixed
  - text response. The overall score is computed as a significantly superior performance obtained by the
  - 鈥 Our model significantly outperforms the Llama-3 terpretability in reward models for RLHF in the
  - 鈥 Our model also outperforms the LLM-as-a-judge wardBench, demonstrating the effectiveness of our
  - improve the interpretability and effectiveness
  - improvement for multi-step reasoning llm agent. Aleksander Madry. 2020. Implementation matters in
  - Zhang, KaShun SHUM, and Tong Zhang. 2023. wards improved safety alignment of LLM via a

## 6. Conclusion、局限与可复现性

- **结论段落线索**：other reward models. Several key observations can be made from these results: In this work, we addressed the critical issue of in- 鈥 Our model significantly outperforms the Llama-3 terpretability in reward models for RLHF in the 8B Bradley-Terry RM, which provides the LLM context of aligning LLMs with human preferences. backbone for our model. This demonstrates the We proposed a novel two-stage approach, consist- effectiveness of our ArmoRM design and the ing of an ArmoRM and a MoE strategy with a gat- MoE gating mechanism in improving the perfor- ing network. Our ArmoRM, trained with Llama-3 mance of reward models. 8B, achieved state-of-the-art performance on Re- 鈥 Our model also outperforms the LLM-as-a-judge wardBench, demonstrating the effectiveness of our approach (Zheng et al., 2023) with GPT-4 judges reward modeling approach. 10586 Acknowledgments introduce biases or reward undesirable behav- iors in the aligned LLMs. This could lead to the HW, HZ, and TZ are partially supported
- **局限/未来工作线索**：Limitations process, may also be exploited by malicious；mark, there are some limitations to consider: LLMs before deploying them in sensitive appli-；4 340B, we were unable to perform experiments Despite these limitations, we believe that our；with models beyond 8B parameters. Future work represents an important step towards build-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Interpretable Preferences via Multi-Objective Reward Modeling and Mixture-of-Experts”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
