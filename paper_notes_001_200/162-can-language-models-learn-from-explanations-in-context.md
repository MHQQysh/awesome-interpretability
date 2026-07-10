# 162. Can language models learn from explanations in context?

> 逐篇阅读记录：第 162 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Andrew K. Lampinen, Ishita Dasgupta, Stephanie C. Y. Chan, Kory W. Mathewson, Mh Tessler, Antonia Creswell, James L. McClelland, Jane Wang, et al.
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.findings-emnlp.38)
- **领域标签**：Rationale, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 16,594 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Andrew Lampinen, Ishita Dasgupta, Stephanie Chan, Kory Mathewson, Mh Tessler, Antonia Creswell, James McClelland, Jane Wang, Felix Hill.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Results；4 Related work answer explanations (even on some tasks like arith-；6 Conclusions clude certain patterns of explanation (or even er-；2021 CHI Conference on Human Factors in Comput-；2022. Training language models to follow instruc- Bach, Lintang Sutawika, Zaid Alyafeai, Antoine；34. Hans-Georg Luigs, Anne-Katrin Mahlein, and Kris-；2022. STaR: Bootstrapping reasoning with reason-；2020. Towards interpretable natural language under-
- **引言关键线索**：A new paradigm has emerged in natural language Practically, it has the potential to improve few-shot processing: few-shot prompting of Language Mod- performance. But more fundamentally, the answer els (LMs). Large LMs appear to exhibit some in- sheds light on the scientific question of what kind context learning abilities, such that they can in- of in-context learning abilities LMs exhibit, which fer how to perform a new language task few-shot is a topic of ongoing debate (e.g. Min et al., 2022; (Brown et al., 2020)鈥攆rom a few examples of in- Webson and Pavlick, 2021). put and output pairs within the model鈥檚 context win- Several converging findings suggest that expla- dow, but without training. For instance, these mod- nations might improve few-shot performance. First, els can answer trivia questions or perform arith- prompting with explicit instructions or task descrip- metic more accur
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：thereby improve downstream task performance. LMs could benefit from explanations with- Answer these questions by identifying instruction out necessarily 鈥渦nderstanding鈥 explanations in a whether the second sentence is an appro- Task human-like way (cf. Mitchell, 2021). Explanations priate paraphrase of the first, metaphori- are prevalent in human discourse, and therefore cal sentence. LMs will have encountered explanations in train- Q: David鈥檚 eyes were like daggers at ing that are intended (by the humans who wrote Paul when Paul invited his new girl- them) to clarify understanding and improve future friend to dance. <- -> David had two example #1 Few-shot reasoning. For example, there are many exam an- daggers when Paul invited his new girl- swer keys online which contain questions, answers, friend to dance. and explanations of those answers that could help choice: True predict the answers to subsequent questions on the choice: False e
- **方法与解释性关系**：该论文主要围绕 `Rationale, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：See App. A for, We were granted evaluation, LMs planatory content that, Fig, As a control, Comparisons between our explanations, Specifically, Baseline score without explanations, Explanation effects by baseline
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - See App. A for all tasks used and selection process. are not due to word-level features, we compared to
  - We were granted evaluation access to a set of LMs planatory content that matters, we compared to a
  - 鈥淓xplanation:鈥 (Fig. 1); this contrasts with prior tial benefits. As a control, we compare to select-
  - text. Comparisons between our explanations and natural to ask whether there are cognitive implica-
  - padimitriou et al., 2022). Specifically, we compare all conditions, after the task instruction and before
  - ment, and so used direct likelihood comparisons

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - examples to task principles can improve learn- shot prompt bears some resemblance to the flexibil-
  - prompts, and models. We find that explana- are contradictory; a number cannot be both prime
  - tions can improve performance鈥攅ven without and divisible by 6, which is composite.鈥). Thus, ex-
  - tions together substantially improves perfor- benefit from explanations when learning from ex-
  - even untuned explanations outperform care- improve few-shot task performance? Note that
  - A new paradigm has emerged in natural language Practically, it has the potential to improve few-shot
  - dow, but without training. For instance, these mod- nations might improve few-shot performance. First,

## 6. Conclusion、局限与可复现性

- **结论段落线索**：We evaluate model performance in each prompt explanations prompt (without instructions), and condition on all task dataset items (except those edited the text of the explanations to try to im- included in the prompt). We restrict to multiple prove performance on the validation set (the 10 choice tasks, and evaluate the model鈥檚 likelihood examples from the other prompts). This process of each answer option after conditioning on the often involved relatively minor editing, e.g., more prompt and question (Fig. 1). We do not normal- 鈥渓anguage-like鈥 or pedagogical explanations (see ize the likelihoods by answer length, but answer App. B.6). Hand-tuning allows us to more tightly lengths are generally similar within a question, and lower-bound the possible effects of more optimal in some preliminary experiments such normaliza- explanations. tion did not improve performance. We greedily Note that in each case above, tuning was per- choose the highest-likelihood answer from the set, formed on a
- **局限/未来工作线索**：direction for future work. Thus, while only the；Limitations largest model we evaluated showed benefits of ex-；cerns. Nevertheless, future work should explore planations are not a panacea. However, just as；user interpretability, though with the caveat that ex-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Can language models learn from explanations in context?”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
