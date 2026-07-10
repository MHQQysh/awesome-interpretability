# 154. DEPN: Detecting and Editing Privacy Neurons in Pretrained Language Models

> 逐篇阅读记录：第 154 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xinwei Wu, Junzhuo Li, Minghui Xu, Weilong Dong, Shuangzhi Wu, Chao Bian, Deyi Xiong
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.174)
- **领域标签**：Attribution, Hidden, LLM, Activation, Detect
- **本地 PDF 文本规模**：约 7,011 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Pretrained language models have learned a vast amount of human knowledge from large-scale corpora, but their powerful memorization capability also brings the risk of data leakage.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：5 Analysis experiments to investigate the relationship between；2021. Generalization in nli: Ways (not) to go be-；2020. Sensitive data detection and classification in Kevin Meng, David Bau, Alex Andonian, and Yonatan；2020. Descent-to-delete: Gradient-based methods；2022. Training language models to follow instruc-；2021. Cape: Context-aware private embeddings for；2021 Conference on Empirical Methods in Natural；16. We fine-tuned BERT-base on the Enron dataset
- **引言关键线索**：post-processing stage, which involve slight param- Remarkable progress has been made in large lan- eter retraining to make the model forget privacy guage models (LLMs) in recent years (Brown et al., information (Bourtoule et al., 2021; Gupta et al., 2020; Liu et al., 2021; Ouyang et al., 2022; Lee 2021; Neel et al., 2020). Nevertheless, these meth- et al., 2023). However,despite this success, LLMs ods generally incur high computational complexity, are confronted with privacy and security concerns making it challenging to apply them to complex in real-world applications (Guo et al., 2022; Brown model architectures. In practice, model develop- et al., 2022; Li et al., 2023). The primary cause of ers often attempt to prevent language models from privacy and security risks is the inherent nature of outputting specific information via blocking or fil- large pretrained language models. Previou
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：der to effectively reduce these risks, we pro- to attack LLMs for training data extraction. For pose a framework DEPN to Detect and Edit instance, Carlini et al. (2021) have successfully ex- Privacy Neurons in pretrained language mod- tracted personal information from GPT-3鈥檚 output, els, partially inspired by knowledge neurons while Li et al. (2023) have induced the genera- and model editing. In DEPN, we introduce a novel method, termed as privacy neuron detec- tion of personal information by utilizing multi-step tor, to locate neurons associated with private prompts in ChatGPT. All these show that large pre- information, and then edit these detected pri- trained language models suffer from a serious risk vacy neurons by setting their activations to zero. of privacy leakage. Furthermore, we propose a privacy neuron ag- In order to safeguard privacy, numerous meth- gregator dememorize private information in a ods have been proposed. The
- **方法与解释性关系**：该论文主要围绕 `Attribution, Hidden, LLM, Activation, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Baselines To demonstrate the, After the privacy attribution, DEPN, BERT-O, The bert model, Table 1, Results of testing the, Phone Numbers, Names, Texts on different baseline, DEPN significantly reduces the, Figure 4, Comparison of privacy leakage
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - bution score matrix of each sequence in the batch. Baselines To demonstrate the effectiveness and
  - After the privacy attribution score calculation, we robustness of DEPN, we compared it with the fol-
  - let each sequence vote for neurons according to lowing baseline models. BERT-O: The bert model
  - Table 1: Results of testing the risks of leaking private Phone Numbers, Names, and Texts on different baseline
  - comparison, DEPN significantly reduces the risk of
  - els 3 to compare the performance of privacy erasure Figure 4: Comparison of privacy leakage risk reduction

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Large language models pretrained on a huge regurgitate a significant portion of the training data,
  - show that our method can significantly and ef-
  - Experimental results show that our framework over the vocabulary. Based on this finding, a strand
  - comparison, DEPN significantly reduces the risk of
  - In conclusion, while differential privacy training leads to a decrease in the model performance.
  - may significantly undermine model performance. Privacy Neurons
  - significantly decreases. In Figure 2(b), the purple the 6-th epoch. This suggests that as the depth

## 6. Conclusion、局限与可复现性

- **结论段落线索**：and fine-tuning with non-private data can mitigate privacy leakage risks, they incur more time and 5.2 Relationship between Memorization And may significantly undermine model performance. Privacy Neurons The DEPN framework strikes an excellent balance As it is widely recognized, privacy data leakage between performance and privacy protection. often stems from the model鈥檚 ability to memorize the training data. In this subsection, we conducted
- **局限/未来工作线索**：However, these methods have a limitation that；In this subsection we discuss the limitation of learning, training model parameters to accommo-；Privacy Protection To address privacy risks in Limitations Our current study still has two lim-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“DEPN: Detecting and Editing Privacy Neurons in Pretrained Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
