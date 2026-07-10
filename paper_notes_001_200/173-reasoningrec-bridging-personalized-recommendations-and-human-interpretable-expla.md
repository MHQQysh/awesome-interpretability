# 173. ReasoningRec: Bridging Personalized Recommendations and Human-Interpretable Explanations through LLM Reasoning

> 逐篇阅读记录：第 173 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Millennium Bismay, Xiangjue Dong, James Caverlee
- **发表 venue / date**：NAACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-naacl.454)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 11,207 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：This paper presents ReasoningRec, a reasoningbased recommendation framework that leverages Large Language Models (LLMs) to bridge the gap between recommendations and humaninterpretable explanations.In contrast to convent
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction tween recommendations and the human-intelligible,；4 Experimental Setup We also consider two zero-shot methods for com-；5 Results and Analysis training samples and is able to outperform all other
- **引言关键线索**：interpretable reasoning behind them? Concretely, we propose a reasoning-based recommendation In our day-to-day life, many of our experiences are framework called ReasoningRec. The core idea driven by word-of-mouth recommendations. For is to model users and items through LLM-powered example, a friend may suggest a good book to read, generation (corresponding to the likes and dislikes knowing my preference for historical fiction and above), and then use a large LLM to generate syn- thriller novels. A work colleague may introduce me thetic explanations for why a user may like an item to a new software library based on our shared work (corresponding to the explanatory reasoning); these history. These recommendations often focus on explanations are then used to fine-tune a smaller likes and dislikes of a user and provide explanatory LLM (SLM) toward generating more accurate rec- reasoning for
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：ages Large Language Models (LLMs) to bridge the gap between recommendations and human- interpretable explanations. In contrast to con- In practice, many modern recommendation sys- ventional recommendation systems that rely on tems aim to scale recommendations based on the ac- implicit user-item interactions, ReasoningRec tions of millions of users, e.g., (Kang and McAuley, employs LLMs to model users and items, fo- 2018; Tang and Wang, 2018; Koren et al., 2009; cusing on preferences, aversions, and explana- He et al., 2017; Yue et al., 2024; Sun et al., 2019). tory reasoning. The framework utilizes a larger These approaches, however, are typically not driven LLM to generate synthetic explanations for user preferences, subsequently used to fine- by such explicit human-interpretable explanatory tune a smaller LLM for enhanced recommen- reasoning, since such detailed reasons are rarely dation accuracy and human-interpretable expla- provide
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Baselines and Evaluation Metrics, In this section, Zero-shot Vanilla is prompted, ML1M, Harper and Konstan, Table 3, Recommendation performance of ReasoningRec, SASRec and, ReasoningRec outperforms all baselines, ReasoningRec, SAS- lightweight instruction finetuning, Hence, Description and User Profile, Ama- A.2 Baselines
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - (sui )t鈭1 t鈭1 4.2 Baselines and Evaluation Metrics
  - In this section, we introduce the datasets, baseline soningRec. Zero-shot Vanilla is prompted with
  - ML1M (Harper and Konstan, 2015) is a widely for these two baselines.
  - Table 3: Recommendation performance of ReasoningRec and baselines across three datasets. SASRec and
  - 5.1 ReasoningRec outperforms all baselines soning generation, showcasing the cost and com-
  - method, ReasoningRec, with baselines like SAS- lightweight instruction finetuning framework with

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - data significantly influences the LLM鈥檚 capac-
  - In our experiments, we find that the ability of the recently, research has shifted towards cost and
  - in improved performance in recommendation pre- improved performance compared to LLMs relying
  - user profiles play a significant role for long user-
  - that ReasoningRec outperforms the state-of-
  - v0.1 as the LLM. outperforms a contemporary work, Rec-SAVER
  - on NVIDIA RTX A5000 24GB GPUs. Additional Improved performance of ReasoningRec could be

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Maciej Besta, Nils Blach, Ales Kubicek, Robert Gersten- In this work, we studied the importance of rich con- berger, Michal Podstawski, Lukas Gianinazzi, Joanna textual and semantic information about users and Gajda, Tomasz Lehmann, Hubert Niewiadomski, Pi- items in generating human-interpretable explana- otr Nyczyk, and Torsten Hoefler. 2024. Graph of tions. We introduced ReasoningRec, a lightweight thoughts: Solving elaborate problems with large lan- guage models. In AAAI, pages 17682鈥17690. AAAI and efficient framework, to instruction finetune an Press. SLM on synthetic reasoning data generated by an LLM, conditioned on ground truth user ratings. We Andrew P. Bradley. 1997. The use of the area under the roc curve in the evaluation of machine learning observed that our proposed method outperformed algorithms. Pattern Recognition, 30(7):1145鈥1159. all the baseline methods in the recommendation prediction task while bridging the gap between rec- Karl Cobbe, Vineet Kosaraju, Mohammad Ba
- **局限/未来工作线索**：input prompt. We assess that, LLMs inherently per- Limitations and Future Work
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“ReasoningRec: Bridging Personalized Recommendations and Human-Interpretable Explanations through LLM Reasoning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
