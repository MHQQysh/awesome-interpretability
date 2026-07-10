# 134. Adversarial Robustness for Large Language NER models using Disentanglement and Word Attributions

> 逐篇阅读记录：第 134 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xiaomeng Jin, Bhanukiran Vinzamuri, Sriram Venkatapathy, Heng Ji, Pradeep Natarajan
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.830)
- **领域标签**：Attribution, Representation, Evaluation, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 9,326 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Large language models (LLM’s) have been widely used for several applications such as question answering, text classification and clustering.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction 2), making it more congenial for a word attribution；2 Related Work；4 Experiments BERT and T5 NER models are provided in the；2020 Conference on Empirical Methods in Natu-
- **引言关键线索**：function like Integrated Gradients (IG) (Sundarara- Named Entity Recognition (NER) aims to iden- jan et al., 2017) to identify diverse, yet important tify and categorize named entities mentioned in words. The other subsequent steps include substi- unstructured text into pre-defined categories such tution of the selected words with entity and context as Person, Location, or Organization. In recent substitution workflows (Figure 4), and selection years, NER tasks (Malmasi et al., 2022) have be- of candidate adversarial examples based on their come more challenging due to the introduction of semantic similarity scores to the original text. complex tagsets, which often leads to the failure Experimental results indicate that to create a of existing NER systems in accurately recognizing successful attack, on average, our method re- these entities. To address this, Large Language quires 69% (de
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：score over original LLM NER model by 8% tanglement (Higgins et al., 2017) is a technique and 18% on CoNLL-2003 and Ontonotes 5.0 datasets respectively. which helps in separating the latent entity and con- text components of an embedding space (Figure
- **方法与解释性关系**：该论文主要围绕 `Attribution, Representation, Evaluation, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：LLM, Specifically, Baseline Methods, We compare our proposed, BLANK, Then, NER adversarial attack baseline, Basically, An attack is 4.4.1, Comparison against baselines, The total pared to, F1 score
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tasks in comparison to fine-tuned pre-trained bustness in current LLM鈥檚 with word-level attacks
  - is clear that in comparison to before disentanglement
  - model f , an input x 鈭 R , and a baseline input
  - suzaki, 2021) to generate texts. Specifically, we 4.2 Baseline Methods
  - mask the context word that needs to be replaced We compare our proposed method with the follow-
  - with [BLANK]. Then, we take the masked sentence ing NER adversarial attack baseline methods:

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - language models (PLM鈥檚). To enhance wider resulting in a significant performance drop of 33%.
  - results based on our method improves the F1 word attribution techniques, respectively. Disen-
  - delivering significant performance improvements
  - sults also demonstrate that our method improves One of the two honorable guests in the school is ......
  - 鈥 We present end-to-end results of improve-
  - three benchmark datasets followed by conclusions.
  - they outperform many NLP tasks and have been tant words using word attributions, (3) Substitution

## 6. Conclusion、局限与可复现性

- **结论段落线索**：a simple NER adversarial attack by perturbing both named entities and contexts in the original texts.
- **局限/未来工作线索**：ues as word importance. (3) Select words using 6 Limitations；a special weighted linear regression to compute One of the key limitations of our work is that we
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Adversarial Robustness for Large Language NER models using Disentanglement and Word Attributions”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
