# 118. MMNeuron: Discovering Neuron-Level Domain-Specific Interpretation in Multimodal Large Language Model

> 逐篇阅读记录：第 118 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jiahao Huo, Yibo Yan, Boren Hu, Yutao Yue, Xuming Hu
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.387)
- **领域标签**：Representation, Lens, MLLM, Hidden, Behavior, Analyze
- **本地 PDF 文本规模**：约 10,498 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决多模态大模型视觉-语言证据如何被使用和对齐难以解释的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Projecting visual features into word embedding space has become a significant fusion strategy adopted by Multimodal Large Language Models (MLLMs).However, its internal mechanisms have yet to be explored.Inspired by multi
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction focused on interpreting the multilingual capabili-；5 Conclusion；1. Our experiments are conducted mainly；2. Although we find that domain-specific infor- Anthony Bau, Yonatan Belinkov, Hassan Sajjad, Nadir；2024. Conceptprune: Concept editing in diffusion matter: Elevating the role of image understanding；2024. Instructblip: Towards general-purpose vision- Junnan Li, Dongxu Li, Caiming Xiong, and Steven
- **引言关键线索**：ties of pre-trained large language models (LLMs) Neuron Analysis, which interprets activation of under the view of language-specific neurons, which neurons as the recall of learned knowledge in deep are neurons uniquely responsible for particular lan- neural networks, has been widely adopted by re- guages. For instance, Kojima et al. (2024) iden- searchers to understand the inner workings of mod- tified such neurons in pre-trained decoder-based els (Sajjad et al., 2022; Fan et al., 2024). Prior stud- language models, demonstrating that tampering ies have confirmed that certain neurons within deep with a few language-specific neurons significantly neural networks play important roles in learning alters the occurrence probability of target language particular concepts (Oikarinen and Weng, 2022; in text generation. Similarly, Zhao et al. (2024c) Bai et al., 2024; Xiao et al., 2024), preserv
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：November 12-16, 2024 漏2024 Association for Computational Linguistics PCA Visualization of Domain-Specific Visual Features Auto Driving terns for these domains. Furthermore, we propose 8 Medical Remote Sensing a three-stage mechanism based on the distribution 6 Common Document of domain-specific neurons among MLLM鈥檚 LLM layers, where post-projection visual features are 4 processed by LLM. To validate our hypothesis, we PCA Component 2 2 employ logit lens (nostalgebraist, 2020) to decode 0 the hidden states of LLM鈥檚 intermediate layers to 鈭2 visualize the feature transformation within trans- former models (Vaswani et al., 2017). 鈭4 Our main contributions are as follows: 鈭6 鈥 We identify the presence of domain-specific 鈭8 鈭6 鈭4 鈭2 0 2 4 6 8 PCA Component 1 neurons in representative MLLMs, which is vital for interpreting domain-specific features. Figure 2: PCA visualization of image embeddings ex- tracted through CLIP鈥檚 image encoder. 鈥 We 
- **方法与解释性关系**：该论文主要围绕 `Representation, Lens, MLLM, Hidden, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Schwettmann et al, We compare features from, MLLMs, Compared with pre-, Q-Former to extract image, BLIP as our baseline, For DocVQA, Baseline Module VQAv2 PMC-VQA, LingoQA DocVQA RS-VQA Baseline, Module VQAv2 PMC-VQA LingoQA, DocVQA RS-VQA, Conference on Empirical Methods, Natural Lee. 2023a. Improved, Prompt Examples Baseline Module, VQAv2 PMC-VQA LingoQA DocVQA, RS-VQA, Encoder Baseline Module VQAv2, PMC-VQA LingoQA DocVQA RS-VQA
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - encoder (Schwettmann et al., 2023). 鈥 We compare features from various domains
  - specific neurons in MLLMs. Compared with pre-
  - employs a Q-Former to extract image features BLIP as our baseline, hoping to bring insights
  - ers into the distribution over the vocabulary, which goQA to make a fair comparison. For DocVQA,
  - Baseline Module VQAv2 PMC-VQA LingoQA DocVQA RS-VQA Baseline Module VQAv2 PMC-VQA LingoQA DocVQA RS-VQA
  - that further, we compare the influence of domain-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - space has become a significant fusion strategy
  - ies have confirmed that certain neurons within deep with a few language-specific neurons significantly
  - way, we find that there exists a large visual gap be- Perturbation for Hidden States We demon-
  - for most domains, we find that deactivating remote the language model鈥檚 intermediate layers into the
  - 2. Although we find that domain-specific infor- Anthony Bau, Yonatan Belinkov, Hassan Sajjad, Nadir
  - 2020 Conference on Empirical Methods in Natural Lee. 2023a. Improved baselines with visual instruc-
  - Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, next: Improved reasoning, ocr, and world knowledge.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：concatenated with language features and fed into MLLMs (Schwettmann et al., 2023; Zhao et al., the model鈥檚 LLM module to generate text outputs. 2024a). Our findings can reveal the neuron-level Specifically, we investigate the activation pat- similarity and distinction among these domains, terns of neurons in MLLMs鈥 feed-forward network offering insights to understand and enhance the (FFN) layers across corpora from five distinct do- cross-domain potential of current MLLMs. mains, identifying less than 1% as domain-specific neurons. The datasets we utilized include Lin- 2 Related Work goQA (Marcu et al., 2023), RS-VQA (HR) (Lo- bry et al., 2020), PMC-VQA (Zhang et al., 2023b), 2.1 Neuron Analysis DocVQA (Mathew et al., 2021) and VQAv2 (Goyal Neuron analysis has been recently widely explored et al., 2017), covering domains such as Auto Driv- in computer vision and natural language process- ing, Remote Sensing, Medicine, Document, and ing, which views neuron activation as the recall of Co
- **局限/未来工作线索**：BLIP on selected domains with corresponding domain- this for future work.；Limitations Liu, Chenfei Wu, Nan Duan, and Vasudev Lal. 2022.；there still exist several limitations: ings of the IEEE/CVF Conference on computer vision；these problems for future work. David Bau, Bolei Zhou, Aditya Khosla, Aude Oliva, and
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“MMNeuron: Discovering Neuron-Level Domain-Specific Interpretation in Multimodal Large Language Model”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
