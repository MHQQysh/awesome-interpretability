# 077. Extracting Latent Steering Vectors from Pretrained Language Models

> 逐篇阅读记录：第 77 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Nishant Subramani, Nivedita Suresh, Matthew E. Peters
- **发表 venue / date**：ACL / 2022/01
- **正式页面**：[Paper](https://aclanthology.org/2022.findings-acl.48)
- **领域标签**：Representation, Steering, Evaluation, Hidden, LLM, Behavior, Control
- **本地 PDF 文本规模**：约 9,581 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Prior work on controllable text generation has focused on learning how to control language models through trainable decoding, smartprompt design, or fine-tuning based on a desired objective.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Extracting Steering Vectors vector-based recovery and style transfer among oth-；9 Ethics Statement；7 Related Work We introduce a new approach for controllable text；2020 Conference on Empirical Methods in Natural
- **引言关键线索**：lying language model at all. Leveraging large pretrained language models Next, we take our extracted steering vectors and trained on massive Web corpora has become the explore whether they can be used for unsupervised go-to approach to solve natural language process- sentiment transfer on the Yelp sentiment bench- ing tasks (Peters et al., 2018; Radford et al., 2018; mark (Zhang et al., 2015). We find that adding an Devlin et al., 2018; Brown et al., 2020). As a result, offset vector to extracted steering vectors performs controlling these models has become paramount as comparably to carefully designed, autoencoder- many applications of NLP technology require con- based models. To see whether steering vectors trol over the generations of the model. Prior work encode semantics, we explore whether they can be aims to learn how to control language models and used for unsupervised textual si
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：a target sentence is already encoded within the ier by learning a latent space that is more easily model. Accordingly, we explore a different manipulated to encourage models to generate text approach altogether: extracting latent vectors corresponding to a target attribute such as positive directly from pretrained language model de- coders without fine-tuning. Experiments show sentiment in the case of sentiment transfer. that there exist steering vectors, which, when We take a more direct approach and explore added to the hidden states of the language whether it is possible to extract latent vectors di- model, generate a target sentence nearly per- rectly from pretrained language model decoders fectly (> 99 BLEU) for English sentences from without fine-tuning. We call these vectors steering a variety of domains. We show that vector arith- vectors and define the latent steering space of a metic can be used for unsupervised sentiment tran
- **方法与解释性关系**：该论文主要围绕 `Representation, Steering, Evaluation, Hidden, LLM, Behavior, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：With as few as, The vertical lines indicate, GPT2-117M and BERT-, Language Processing, Copen-, Daan van Esch, Nasanbayar Ulzii-Orshikh, Al- sentence embeddings, Side-tuning, AAR, ASR, LFS, MFS, This could perhaps be
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - other baselines. With as few as 10 examples per different layers at the first timestep for those sentences.
  - class, performance of our method remains compet- The vertical lines indicate extractive baselines: mean-
  - itive with autoencoder-based baselines. pooled final hidden states for GPT2-117M and BERT-
  - simple but tough-to-beat baseline for sentence em- ural Language Processing, pages 670鈥680, Copen-
  - hab, Daan van Esch, Nasanbayar Ulzii-Orshikh, Al- sentence embeddings: A strong but simple baseline.
  - comparison of smoothing techniques for sentence- using monolingual corpora in neural machine trans-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - a variety of domains. We show that vector arith-
  - this task. We find that distances between steer- generate that sentence exactly. During decoding,
  - (STS-B), outperforming pooled hidden states Rather than training a model to learn steering vec-
  - Taken together, our results suggest that frozen
  - ing tasks (Peters et al., 2018; Radford et al., 2018; mark (Zhang et al., 2015). We find that adding an
  - Code is available at https://github.com/nis et al. (2017)), our steering vectors outperform ex-
  - Also, we find that our methods do not simply mem- representation, we ask whether its possible to ex-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：attend to these zsteer vectors. Experiments on the books subset show that recovery is much lower In this paper we introduce a different approach with this prompt-based approach than when inject- to controllable text generation, where we extract ing steering vectors directly into the transformer latent steering vectors directly from a pretrained stack of the model. Even with k = 50 steering language model without fine-tuning. Further, we vectors injected via this prompt-based approach, find that our steering vectors lead to near perfect recovery fails to match that of a single steering recovery on English sentences from a variety of vector zsteer injected into the hidden states of the domains. We show that vector arithmetic can be language model. used in the context of unsupervised style transfer on the Yelp sentiment dataset and StylePTB bench- Num prompt 1 5 10 20 50 mark, performing comparably to models tailored vectors Reconstruction to these tasks. Experiments reveal that distances
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Extracting Latent Steering Vectors from Pretrained Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
