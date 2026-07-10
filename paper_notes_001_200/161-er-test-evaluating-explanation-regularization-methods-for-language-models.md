# 161. ER-Test: Evaluating Explanation Regularization Methods for Language Models

> 逐篇阅读记录：第 161 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Brihi Joshi, Aaron Chan, Ziyi Liu, Shaoliang Nie, Maziar Sanjabi, Hamed Firooz, Xiang Ren
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.findings-emnlp.242)
- **领域标签**：Rationale, Evaluation, LLM, Behavior, Explain
- **本地 PDF 文本规模**：约 16,269 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：By explaining how humans would solve a given task, human rationales can provide strong learning signal for neural language models (NLMs).
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：4 ER-T EST Design Choices；1 Pn t；6 Related Work；8 Acknowledgments are released publicly for usage and have been duly；2020. Evaluating and characterizing human ratio-；2014. Extraction of salient sentences from labelled
- **引言关键线索**：Rieger et al., 2020; Liu and Avci, 2019). Human ra- Neural language models (NLMs) have achieved tionales can be created by annotating each training state-of-the-art performance on a broad array of instance individually (Lin et al., 2020; Camburu natural language processing (NLP) tasks (Devlin et al., 2018; Rajani et al., 2019) or by applying et al., 2018; Liu et al., 2019). Even so, NLMs鈥 rea- task-level human priors across all training instances soning processes are notoriously opaque (Rudin, (Rieger et al., 2020; Ross et al., 2017; Liu and Avci, 鈭 Equal contribution. 2019). 3315 Findings of the Association for Computational Linguistics: EMNLP 2022, pages 3315鈥3336 December 7-11, 2022 漏2022 Association for Computational Linguistics Though prior works primarily evaluate ER mod- pushing the NLM鈥檚 (extractive) machine rationales els鈥 in-distribution (ID) generalization, the results (Which 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Brihi Joshi鈾ｂ垪 Aaron Chan鈾ｂ垪 Ziyi Liu鈾ｂ垪 Shaoliang Nie鈾 Maziar Sanjabi鈾 Hamed Firooz鈾 Xiang Ren鈾 鈾 University of Southern California 鈾 Meta AI {brihijos, chanaaro, zliu2803, xiangren}@usc.edu {snie, maziars, mhfirooz}@fb.com Abstract By explaining how humans would solve a given task, human rationales can provide strong learning signal for neural language models (NLMs). Explanation regularization (ER) aims to improve NLM generalization by pushing the NLM鈥檚 machine rationales (Which input tokens did the NLM focus on?) to align with human rationales (Which input tokens would humans Figure 1: Explanation Regularization (ER). ER improves focus on?). Though prior works primarily study model generalization on NLP tasks by pushing the model鈥檚 ER via in-distribution (ID) evaluation, out-of- machine rationales (Which input tokens did the model focus distribution (OOD) generalization is often more on?) to align with human rationales (Which input t
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, LLM, Behavior, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Transformer encoder, Also, FNo-ER denote an NLM, First, Results, Tables 1 and 2, Figure 4 summarize, Sec, Third, None baseline, SST and eSNLI, Macro F1, NLI, Cells highlighted in blue, Order has the research, Setup, For experiments in this, Table 4 demonstrates that
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - comparison of different works using ER, little is un- et al., 2018), consisting of a Transformer encoder
  - yields more improvements than label annotation, Also, as a baseline, let FNo-ER denote an NLM that
  - First, we compare different rationale alignment Results. Tables 1 and 2, and Figure 4 summarize
  - Sec. 5.3) Third, we look into strategies on how as the None baseline (for both SST and eSNLI).
  - analysis and Macro F1 (鈫) for NLI. Cells highlighted in blue show significant improvement over the None baseline. (p < 0.05)
  - sentiment analysis, we observe that Order has the research question, we compare and contrast differ-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - to improve NLM generalization by pushing the
  - Figure 1: Explanation Regularization (ER). ER improves
  - which has spurred significant interest in designing
  - datasets, we show that ER has little impact on 2019; Lundberg and Lee, 2017; Chan et al., 2022).
  - task-dependent. Also, ER can improve OOD rithms can be utilized to improve NLM decision-
  - man rationales. Finally, we find that rationale
  - results with ER-T EST help demonstrate ER鈥檚 tion (ER), which aims to improve NLM by regu-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：In this work, we study explanation regularization Current work simulates human rationales in (ER) 鈥 aligning machine rationales with human ra- the ER pipeline. ER is meant to align machine- tionales, in detail. We propose ER-T EST, that eval- generated rationales with human-rationales. How- uates ER鈥檚 OOD generalization along three pillars ever in our current work, we use human rationales - unseen datasets, contrast set tests and functional that are pre-annotated as part of the datasets we tests, and uses it to investigate four research ques- use. This simulation of live human feedback is tions surrounding the choice of the rationale align- used in rationale alignment criterion. We believe ment criterion, type of human rationale, choice of this limitation can be easily addressed, by collect- and time taken to obtain rationale-annotation in- ing human-in-the-loop rationale annotations. stances. Although ER shows minor impact on ID task performance, improvements on OOD datasets Current w
- **局限/未来工作线索**：2019). Carton et al. (2022) showed that maximiz- 9 Limitations；7 Conclusion and Future Work generate for new tasks and datasets.；this limitation can be easily addressed, by collect-；but plan to add other datasets in future work. poor ER optimization. Based on these results, we
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“ER-Test: Evaluating Explanation Regularization Methods for Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
