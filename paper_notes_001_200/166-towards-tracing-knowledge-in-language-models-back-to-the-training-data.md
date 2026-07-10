# 166. Towards Tracing Knowledge in Language Models Back to the Training Data

> 逐篇阅读记录：第 166 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ekin Akyürek, Tolga Bolukbasi, Frederick Liu, Binbin Xiong, Ian Tenney, Jacob Andreas, Kelvin Guu
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.findings-emnlp.180)
- **领域标签**：Attribution, Representation, Evaluation, Hallucination, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 10,418 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Language models (LMs) have been shown to memorize a great deal of factual knowledge contained in their training data.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Fact Tracing Datasets；5 Results bution methods, but rather to assess one expected；5 True A: 1:entity-3468, 2:entity-4097 A: 1:MXXX-entity, 2: 506-
- **引言关键线索**：tually incorrect statements (Lee et al., 2018; Tian Research has shown that language models (LMs) et al., 2019), which is problematic for many appli- acquire significant amounts of world knowledge cations where trustworthiness is important. Hence, from the massive text corpora on which they are there is an urgent need to understand exactly how trained (Petroni et al., 2019; Raffel et al., 2020). LMs acquire and store knowledge so that we may This development has enabled exciting advances improve their accuracy and coverage. 1 Code for the experiments is released at https://github.com/ekinakyurek/influence, Training Data Attribution Ultimately, a lan- and the datasets can be downloaded from https: //huggingface.co/datasets/ekinakyurek/ftrace. guage model鈥檚 鈥渒nowledge鈥 must derive from its Correspondences to akyurek@mit.edu training data. But there has been little research 2429 Findings of
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：鈥 gradient-based and embedding-based 鈥 and Figure 1: FTRACE benchmark for tracing a language find that much headroom remains. For example, model鈥檚 predictions back to training examples (鈥減ro- both methods have lower proponent-retrieval ponents鈥): We provide two fact attribution datasets: precision than an information retrieval baseline one with real facts (FTRACE-TRE X) and one with (BM25) that does not have access to the LM synthetic facts (FTRACE-S YNTH). We evaluate com- at all. We identify key challenges that may monly studied attribution methods, including gradient- be necessary for further improvement such as based and embedding-based approaches for their ability overcoming the problem of gradient saturation, to identify true proponents. and also show how several nuanced implemen- tation details of existing neural TDA methods in knowledge-intensive NLP tasks such as open- can significantly improve overall fact tracing performance.
- **方法与解释性关系**：该论文主要围绕 `Attribution, Representation, Evaluation, Hallucination, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：FTRACE-TRE X, TDA methods against a, BM25, Baseline, BM25 truth proponents from, The ex-, Each, First, CMXCVII-entity is the writ-, We call, IR baselines, To evaluate T RAC, Equa- bel, ANDOM -TARGET baseline, This baseline is indeed
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - precision than an information retrieval baseline one with real facts (FTRACE-TRE X) and one with
  - TDA methods against a simple baseline: BM25
  - widely used information retrieval baseline, BM25,
  - 2.3 Baseline: BM25 truth proponents from the attribution set. The ex-
  - ing variant, has been consistently used as a baseline (Br眉mmer et al., 2016) abstracts, ai 鈭 A. Each
  - the exact positions where the subject and object baselines such as BM25: First, many of the facts

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：81%, 2.5x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - be necessary for further improvement such as based and embedding-based approaches for their ability
  - can significantly improve overall fact tracing
  - acquire significant amounts of world knowledge
  - improve their accuracy and coverage.
  - we improve them by introducing a novel way of objective evaluated at the final converged model
  - and Stern, 2018). Beside the improvements and derivation). In this form, influence functions can be
  - upper-bound on neural TDA methods significantly Liang (2017), the cost is still too high to directly

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ating on predictions we know the LM gets right. We introduced a new dataset and benchmark for Language Models as Retrievers Language mod- fact tracing: the task of tracing language models鈥 els have been successfully used in numerous IR assertions back to the training examples that pro- applications. Karpukhin et al. (2020) use language vided evidence for those predictions. We evalu- model embeddings to warm-start neural retrievers ated gradient-based and embedding-based TDA for knowledge-intensive tasks. Guu et al. (2020) methods and found that they perform worse than a and Lewis et al. (2020) show that language model- standard IR baseline (BM25) even in settings that ing and information retrieval can be jointly learned favor TDA methods. We investigated the effects of in a manner that benefits both tasks. Our work layer selection, model checkpoints and fine-tuning. uses TDA-based retrieval methods to help users Our ablative analysis suggests that saturation is an understand the behavi
- **局限/未来工作线索**：do not necessarily involve fine-grained factual in- There are two caveats for the above setup. First,；into the LM). With a few caveats, we now know In this way, a TDA method always has the oppor-；Limitations and Impact Statement deep bidirectional transformers for language under-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Towards Tracing Knowledge in Language Models Back to the Training Data”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
