# 054. SPIN: Sparsifying and Integrating Internal Neurons in Large Language Models for Text Classification

> 逐篇阅读记录：第 54 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Difan Jiao, Yilun Liu, Zhenwei Tang, Daniel Matter, Jürgen Pfeffer, Ashton Anderson
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.277)
- **领域标签**：Analysis, Hidden, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 10,701 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，通过表征分析、因果干预或解释评估来连接内部机制与外部行为。
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction In this work, we introduce SPIN: a model-；2023. Chatgpt outperforms crowd workers for；2020. New frontiers in mining complex patterns:
- **引言关键线索**：agnostic framework that sparsifies and integrates Large Language Models (LLMs) have achieved internal neurons of intermediate layers of LLMs for state-of-the-art performance in a wide spectrum of text classification. As shown in Figure 1, instead of important tasks, including text classification such relying solely on the final layer鈥檚 hidden states, our as sentiment analysis (Srivastava et al., 2022). Al- method uses internal representations (feed-forward though prompting methods (Wei et al., 2022; Ko- network activations and hidden states) as multi- jima et al., 2022) have gained popularity in de- layered features to enhance the classification head. ploying LLMs for text classification, employing These raw internal representations require further a classification head with these models remains processing before being utilized, as internal neu- a dominant paradigm, mainly due to its sup
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：tained in internal neurons largely untapped. In ity, since it treats models as black boxes. As the 1 this study, we present SPIN : a model-agnostic demand grows for models that not only perform framework that sparsifies and integrates inter- well but are also interpretable and cost-efficient to nal neurons of intermediate layers of LLMs for train and run, moving beyond the current paradigm text classification. Specifically, SPIN sparsifies is becoming increasingly important. internal neurons by linear probing-based salient neuron selection layer by layer, avoiding noise We have reason to believe that delving into the from unrelated neurons and ensuring efficiency. internal of LLMs would bear fruit. As recent stud- The cross-layer salient neurons are then inte- ies in AI interpretability (Radford et al., 2017; Bills grated to serve as multi-layered features for the et al., 2023; Gurnee and Tegmark, 2023) have re- classification head. Ext
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hidden, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Figure 1, Overview of, SPIN that uses sparsified, LLMs for text classification, As shown in Fig-, Baselines, As shown in Eq, Experiments these baseline methods, Base, It is important to, For IMDb and EDOS, For performance comparisons, Table 1, Performance of SPIN and, LLMs, SPIN is compatible with, SPIN, Flan-T5-S
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Figure 1: Overview of (a) baseline method that only uses the terminal hidden states; (b) SPIN that uses sparsified
  - ble to state-of-the-art baseline methods that involve of LLMs for text classification. As shown in Fig-
  - pooled hidden states of the final layer as frozen Baselines. As shown in Eq.(7), a classification
  - 3 Experiments these baseline methods as Base.
  - It is important to note that the baseline classifi-
  - For IMDb and EDOS, we use the provided dataset A.2. For performance comparisons, we adhere to

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：2
                                                                       times, 2 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significantly improve text classification accu-
  - fine-tuning LLMs, accomplished by significantly ure 1 (b), SPIN first sparsifies internal neurons with
  - improved training efficiency. This is achieved by linear probing-based salient neuron selection to ex-
  - LLM layers, significantly speeding up the inference The internal representations from all layers of a
  - strate SPIN鈥檚 superior performance, improved geted task.
  - mulative contribution of the most significant neu-
  - model performance (SoTA) for each dataset. %impr. denotes percentages of improvement. The best results (except

## 6. Conclusion、局限与可复现性

- **结论段落线索**：understandings across their layered architecture, about implementing SPIN for multiclass classifica- and the internal neurons inherently encapsulate a tion tasks is detailed in Appendix C.2. wealth of information. Following the sparsifica- Language Models. We consider representative tion of internal neurons from each respective layer, pre-trained LLMs of mainstream architectures, we proceed to integrate them as cross-layer multi- including encoder-based models (BERT vari- grained representations into the classification head: ants), decoder-based models (GPT-2 variants), and N encoder-decoder models (T5 variants). As contem- 1 min 鈭 L(yi , 蟽(W xi + b)), (7) porary LLMs continue to scale in size, we demon- W ,b N strate SPIN鈥檚 scalability on LLaMA2-7B and 13B i=1 in Appendix C.3. Conventional LLM-based text classifiers regard the pooled hidden states of the final layer as frozen Baselines. As shown in Eq.(7), a classification th features of the i input sentence si , i.e., the textual hea
- **局限/未来工作线索**：Limitations remarks and content. Researchers must employ；One of the primary limitations of the SPIN frame- parency when working with these datasets.；overcomes this limitation by effectively utilizing
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“SPIN: Sparsifying and Integrating Internal Neurons in Large Language Models for Text Classification”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
