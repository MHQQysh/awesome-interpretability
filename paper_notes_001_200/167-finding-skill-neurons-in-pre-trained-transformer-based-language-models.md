# 167. Finding Skill Neurons in Pre-trained Transformer-based Language Models

> 逐篇阅读记录：第 167 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xiaozhi Wang, Kaiyue Wen, Zhengyan Zhang, Lei Hou, Zhiyuan Liu, Juanzi Li
- **发表 venue / date**：EMNLP / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.emnlp-main.765)
- **领域标签**：Analysis, Hidden, LLM, Activation, Analyze
- **本地 PDF 文本规模**：约 11,512 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Transformer-based pre-trained language models have demonstrated superior performance on various natural language processing tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：102 Positive；2 Preliminary；3 Finding Skill Neurons；2000 Ours QNLI；70 Random；6 Application；8 Conclusion and Future Work the Institute for Guo Qiang, Tsinghua University；2020. Compressing bert: Studying the effects of
- **引言关键线索**：Pre-trained language models (PLMs), mostly based et al., 2022). In this paper, we find that after prompt on Transformer architecture (Vaswani et al., 2017), tuning for a task, the activations on soft prompts of have achieved remarkable performance on broad some neurons within pre-trained Transformers are and diverse natural language processing (NLP) highly predictive for the task. For instance, Fig- tasks (Han et al., 2021). However, it remains un- ure 1 shows the activation distribution of a specific clear how the skills required to handle these tasks neuron within RoBERTaBASE (Liu et al., 2019b). distribute among model parameters. Are there This neuron鈥檚 activation is highly predictive of the 鈭 labels of SST-2 (Socher et al., 2013), an established indicates equal contribution. 鈥 sentiment analysis dataset. When the input sen- Corresponding author: Z.Liu and L.Hou. 1 For brevity, Transf
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：such as the adapter-based tuning and BitFit. We 2018; Mitchell et al., 2021), and improve model also explore the applications of skill neurons, efficiency (Dalvi et al., 2020; Zhang et al., 2021). including accelerating Transformers with net- work pruning and building better transferability Prompt tuning (Li and Liang, 2021; Lester et al., indicators. These findings may promote fur- 2021) prepends some trainable embeddings, i.e., ther research on understanding Transformers. soft prompts, into the inputs and adapts PLMs to The source code can be obtained from https: handle tasks by only tuning the soft prompts while //github.com/THU-KEG/Skill-Neuron. freezing all the PLM parameters. It has attracted wide attention recently as a promising parameter-
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hidden, LLM, Activation, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：If this neuron, Transformer to 66.6, Geva et al
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - neuron鈥檚 baseline activation. If this neuron鈥檚 activa- skill neurons active, we can reduce the pre-trained
  - tion for an input sample is higher than the baseline, Transformer to 66.6% of its original parameters
  - calculate the baseline activation of N on pi over
  - which is also the case of previous work (Geva et al., lines indicate baseline activations of the two neurons.
  - the 98% frozen neurons always as their baseline

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1          X, 1 X, 6 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - model parameters. In this paper, we find that
  - a task significantly drop when corresponding
  - such as the adapter-based tuning and BitFit. We 2018; Mitchell et al., 2021), and improve model
  - Pre-trained language models (PLMs), mostly based et al., 2022). In this paper, we find that after prompt
  - dation set of multiple soft prompts as the neuron鈥檚 indicators following Su et al. (2021). We improve
  - For multi-class classification tasks, we decompose achieves significantly better performance.
  - drop much more significantly than when random

## 6. Conclusion、局限与可复现性

- **结论段落线索**：active for each task and set the activations of We discuss this with experiments in appendix H. the 98% frozen neurons always as their baseline activations. Considering that the frozen neurons Analyzing Pre-trained Transformers After the are fixed, we merge them into bias terms. We success of Transformer-based PLMs (Devlin et al., apply this pruning method to the top 9 layers 2019; Yang et al., 2019; Raffel et al., 2020), many of RoBERTaBASE and reduce it to 66.6% of its efforts have been devoted to analyzing how PLMs original parameters. The performances of prompt work, such as probing the knowledge of PLMs (Liu tuning on pruned models and vanilla prompt tuning et al., 2019a; Hewitt and Manning, 2019; Petroni on the original model are shown in Table 4. Our et al., 2019) and understanding the behaviors of pruning based on skill neurons generally performs PLMs鈥 parameters (Voita et al., 2019; Clark et al., comparably to vanilla prompt tuning and can 2019). Among these, some works (Dalvi
- **局限/未来工作线索**：neurons. We encourage future work to explore the ify this, we rank neurons in descending orders of；specific skills may bring noisy signals. Here we neurons encoding information in future works.；8 Conclusion and Future Work the Institute for Guo Qiang, Tsinghua University；Limitations Proceedings of ISWC/ASWC, pages 722鈥735.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Finding Skill Neurons in Pre-trained Transformer-based Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
