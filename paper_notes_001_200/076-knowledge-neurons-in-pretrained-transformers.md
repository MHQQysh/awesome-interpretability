# 076. Knowledge Neurons in Pretrained Transformers

> 逐篇阅读记录：第 76 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Damai Dai, Li Dong, Yaru Hao, Zhifang Sui, Baobao Chang, Furu Wei
- **发表 venue / date**：ACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.acl-long.581)
- **领域标签**：Attribution, Hallucination, Hidden, Activation, Control
- **本地 PDF 文本规模**：约 6,768 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Large-scale pretrained language models are surprisingly good at recalling factual knowledge presented in the training corpus In this paper, we present preliminary studies on how factual knowledge is stored in pretrained
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction attempt to look deeper into pretrained Transformers；3 Identifying Knowledge Neurons where；2019. COMET: commonsense transformers for au- Yihe Dong, Jean-Baptiste Cordonnier, and Andreas；2019 Conference on Empirical Methods in Natu- 57th Conference of the Association for Computa-；2020 Conference on Empirical Methods in Natural
- **引言关键线索**：and investigate how factual knowledge is stored. Large-scale pretrained Transformers (Devlin et al., As shown in Figure 1, we propose a knowl- 2019; Liu et al., 2019; Dong et al., 2019; Clark edge attribution method to identify the neurons et al., 2020; Bao et al., 2020) are usually learned that express a relational fact, where such neurons with a language modeling objective on large-scale are named knowledge neurons. Specifically, we corpora, such as Wikipedia, where exists oceans view feed-forward network (i.e., two-layer percep- of factual knowledge. Pretrained language models tron) modules in Transformer as key-value memo- naturally play as a free-text knowledge base by pre- ries (Geva et al., 2020). For the example in Figure 1, dicting texts (Bosselut et al., 2019). Petroni et al. the hidden state is fed into the first linear layer (2019) and Jiang et al. (2020b) probe factual knowl
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Hidden State express the fact. We find that the activation Neurons of such knowledge neurons is positively cor- related to the expression of their correspond- ing facts. In our case studies, we attempt to Self-Attention Layer leverage knowledge neurons to edit (such as update, and erase) specific factual knowledge 鈥 without fine-tuning. Our results shed light Figure 1: Through knowledge attribution, we identify on understanding the storage of knowledge knowledge neurons that express a relational fact. within pretrained Transformers. The code is available at https://github.com/ Hunter-DDM/knowledge-neurons. text-form knowledge prediction. In this paper, we
- **方法与解释性关系**：该论文主要围绕 `Attribution, Hallucination, Hidden, Activation, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Type of Neurons Ours, Baseline, For a fair comparison, PARA R EL parameters, We show some example, It is motivated by, FFNs, In order to guarantee, Kovaleva, Our baseline method takes, Tenney et al, Geva et al, For the baseline, Their same order of, For comparison
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - we set the attribution threshold t to 0.2 times the Type of Neurons Ours Baseline
  - knowledge neurons. For a fair comparison, we
  - in-the-blank cloze task based on the PARA R EL parameters t and p% for the baseline to ensure
  - et al., 2018). We show some example templates sonable baseline. It is motivated by FFNs鈥檚 analogy
  - entity as a blank to predict. In order to guarantee ally used as a strong attribution baseline (Kovaleva
  - Our baseline method takes the neuron activation Tenney et al. (2019) and Geva et al. (2020).

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：0.2 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - express the fact. We find that the activation Neurons
  - without fine-tuning. Our results shed light Figure 1: Through knowledge attribution, we identify
  - we find that knowledge neurons of a fact tend to most popular and effective NLP architectures. A
  - and tail entities, respectively. Owing to the page width, we show only three templates for each relation. Prompt
  - et al., 2018). We show some example templates sonable baseline. It is motivated by FFNs鈥檚 analogy
  - blank in each prompt with the correct answer for better readability. Owing to the page width, we show only key
  - change. The knowledge erasing operation significantly affects the erased relation, and has just a moderate influence

## 6. Conclusion、局限与可复现性

- **结论段落线索**：et al. (2019) propose to retrieve knowledge in pre- trained models (such as BERT) using cloze queries. We propose an attribution method to identify knowl- Their experiments show that BERT has a strong edge neurons that express factual knowledge in pre- ability to recall factual knowledge without any fine- trained Transformers. We find that suppressing or tuning. Jiang et al. (2020b) improve the cloze amplifying the activation of knowledge neurons queries with mining-based and paraphrasing-based can accordingly affect the strength of knowledge methods. Roberts et al. (2020) propose the closed- expression. Moreover, quantitative and qualitative book question answering to measure how much analysis on open-domain texts shows that knowl- knowledge a pretrained model has stored in its pa- edge neurons tend to be activated by the corre- rameters. Elazar et al. (2021) measure and improve sponding knowledge-expressing prompts. In addi- the consistency of pretrained models with respect tion, we 
- **局限/未来工作线索**：correct answer, corresponding to the manipulation. future work.；leaving precise fact editing for future work.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Knowledge Neurons in Pretrained Transformers”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
