# 199. Visual Chain-of-Thought Prompting for Knowledge-Based Visual Reasoning

> 逐篇阅读记录：第 199 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Zhenfang Chen, Qinhong Zhou, Yikang Shen, Yining Hong, Zhiqing Sun, Dan Gutfreund, Chuang Gan
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/27888/27801)
- **领域标签**：Probing, Rationale, Evaluation, MLLM, Reasoning, Behavior, Analyze
- **本地 PDF 文本规模**：约 8,289 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Knowledge-based visual reasoning remains a daunting task since it not only requires machines to interpret the concepts and relationships from visual scenes but also associate them with external world knowledge to conduct
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：I Describe Model；2019. Ok-vqa: A visual question answering benchmark re-
- **引言关键线索**：form step-by-step logical reasoning to arrive at an answer. Machine visual reasoning has taken a huge step forward since Knowledge-based visual reasoning is more challenging than neuro-symbolic mechanisms (Yi et al. 2018) were introduced, traditional visual question answering and reasoning (Antol which enable machines to develop a reasoning chain with et al. 2015; Goyal et al. 2017) in that the visual context, ex- multiple steps. However, as foreshadowed by cognitive scien- ternal knowledge, and natural language questions need to be tists in early works, such systems of logic and symbols are interactively integrated throughout the reasoning chain. As fundamentally ill-suited to representation and reasoning with depicted in Fig 1, to answer the question 鈥淲hat is the name of real-world, common-sense knowledge (Oaksford and Chater the room?鈥, humans need first to see the room and extract vi
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：until we arrive at a satisfying answer. If machines are also en- visual reasoning, which is able to iteratively attend to the dowed with such capabilities, they can be utilized for numer- related visual concepts in the image and provide consistent ous real-world applications such as assistive robots (Brohan supporting rationales for the answer prediction. VCTP enjoys et al. 2022), and embodied chatbots (Konstantopoulos 2010). several advantages. First, it is effective. Extensive experi- Consistent with cognitive scientists鈥 notion that knowl- ments on knowledge-based benchmarks found that VCTP edge with soft constraints is eminently compatible with large achieves better performance than previous few-shot baselines. connectionist models (Oaksford and Chater 2007), large Moreover, VCTP is more transparent and interpretable since language models (LLMs) indeed have made tremendous it maintains the whole step-by-step reasoning trace that lea
- **方法与解释性关系**：该论文主要围绕 `Probing, Rationale, Evaluation, MLLM, Reasoning, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Our code is available, In particular, Wei cient compared with, Our code would be, Finally, LLM, KB-VR datasets, LLMs for, Manning 2019, Compared with existing learning-in-context, Visual Baselines. We mainly, Genome, Krishna et al. 2017, We select BLIP, Li et al. 2022, PICa, Yang et al. 2022, LLM with in-context example
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - ing baselines; 2). it enjoys the total transparency and trustwor-
  - pared with other fine-tuning baselines. Our code is available at
  - edge with soft constraints is eminently compatible with large achieves better performance than previous few-shot baselines.
  - et al. 2022). In particular, chain-of-thought prompting (Wei cient compared with finetuning methods. Our code would be
  - classifier. Finally, the selected rationale is fed to the LLM鈥檚 on KB-VR datasets, compared with models with LLMs for
  - and Manning 2019), compared with end-to-end neural net- Compared with existing learning-in-context meth-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：5 times, 14 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and drawing the conclusion 鈥淪ofa and coffee table are usu- answers from two consequent iterations are the same.
  - tically related to the query question. We show the prompting 16: end if
  - category names. The LLM selects the most related object to be found in Fig. 3 (B). A significant problem of the LLM鈥檚
  - the most popular knowledge-based VQA dataset with 14,055 supervised methods have a more significant performance dis-
  - formance of our method increases significantly, indicating Ours-BLIP2-Codex鈰 53.2 53.8 56.2
  - inconsistent with visual context (鈥淭he wall is used for a which indicates that our method still outperforms PICa and
  - sofa.鈥), which leads to a wrong answer. CoT significantly after considering the computational cost is-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ally located in the living room鈥. The vision-to-language and To summarize, we introduce VCTP, a novel modularized, language-to-vision interactions can be iteratively performed interactive, and iterative framework for knowledge-based until we arrive at a satisfying answer. If machines are also en- visual reasoning, which is able to iteratively attend to the dowed with such capabilities, they can be utilized for numer- related visual concepts in the image and provide consistent ous real-world applications such as assistive robots (Brohan supporting rationales for the answer prediction. VCTP enjoys et al. 2022), and embodied chatbots (Konstantopoulos 2010). several advantages. First, it is effective. Extensive experi- Consistent with cognitive scientists鈥 notion that knowl- ments on knowledge-based benchmarks found that VCTP edge with soft constraints is eminently compatible with large achieves better performance than previous few-shot baselines. connectionist models (Oaksford and Chater 
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Visual Chain-of-Thought Prompting for Knowledge-Based Visual Reasoning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
