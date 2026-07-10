# 051. Language-Specific Neurons: The Key to Multilingual Capabilities in Large Language Models

> 逐篇阅读记录：第 51 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Tianyi Tang, Wenyang Luo, Haoyang Huang, Dongdong Zhang, Xiaolei Wang, Xin Zhao, Furu Wei, Ji-Rong Wen
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.309)
- **领域标签**：Analysis, Hidden, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 10,755 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Tianyi Tang, Wenyang Luo, Haoyang Huang, Dongdong Zhang, Xiaolei Wang, Xin Zhao, Furu Wei, Ji-Rong Wen.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction alignment capabilities across languages despite the；4 Related Work the exploration of language-specific components；5 Conclusion strategies to harness these identified neurons for；2023. Towards a common understanding of con- Bashlykov, Soumya Batra, Prajjwal Bhargava, Shruti
- **引言关键线索**：absence of multilingual parallel corpora. They The brain has its own language for testing the have identified several critical factors that influ- structure and consistency of the world. ence cross-lingual transfer, including training data Carl Sagan (e.g., overlapped tokens) and training settings (e.g., The pursuit of multilingual capabilities, mirror- shared parameters) (Dufter and Sch眉tze, 2020; ing our world鈥檚 linguistic diversity, is a critical Philippy et al., 2023). Nevertheless, the underlying research objective that paves the way for informa- mechanisms by which the model itself process di- tion democratization across linguistic divides. The verse languages at the composition level continue emergence of pre-trained language models (PLMs) to be an area of vigorous investigation. such as mBERT (Devlin et al., 2019) and XLM- To develop a deeper understanding of the mul- R (Conneau 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：vation probability entropy (LAPE), to identify circles denote activated neurons. When Chinese-specific language-specific neurons within LLMs. Based neurons are deactivated (denoted by 鈯), the model may on LAPE, we conduct comprehensive experi- produce outputs in English. ments on several representative LLMs, such as LLaMA-2, BLOOM, and Mistral. Our findings indicate that LLMs鈥 proficiency in processing a particular language is predominantly due to Furthermore, large language models (LLMs), such a small subset of neurons, primarily situated as GPT-4 (Achiam et al., 2023) and PaLM-2 (Anil in the models鈥 top and bottom layers. Further- et al., 2023), have recently demonstrated more ex- more, we showcase the feasibility to 鈥渟teer鈥 the cellent multilingual capabilities in language under- output language of LLMs by selectively activat- standing, reasoning, and generation, despite being ing or deactivating language-specific neurons. predominan
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hidden, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Zhao et al, We utilize a baseline, Identification methods. For comparison, Specially
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - zh 0.72 5.17 0.88 1.27 0.23 0.53 1.98 zh 0.07 0.10 0.08 0.08 0.03 0.13 0.05 work of Zhao et al. (2023a), we compare the model
  - ciency and avoid confounding variables. We utilize a baseline to randomly select neurons for each lan-
  - Identification methods. For comparison, we specific regions. Specially, for all the compari-
  - row is baseline scores without deactivation while the

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - more, we showcase the feasibility to 鈥渟teer鈥 the cellent multilingual capabilities in language under-
  - R (Conneau et al., 2020a) has marked a significant tilingual capabilities of LLMs, we draw inspira-
  - a particular language can be significantly impacted
  - language-specific neurons significantly impairs the
  - a significant increase in the PPL on French in Fig- compute the mean sentence embedding similarity
  - (Translation: How can I improve my time management
  - sue (Gu et al., 2019; Sennrich et al., 2023). We improve your results. 3. Resist distractions: When you

## 6. Conclusion、局限与可复现性

- **结论段落线索**：enhancing the model鈥檚 multilingual proficiency is Despite the impressive multilingual capabilities still worth exploring. demonstrated by LLMs, the understanding of how these abilities develop and function remains nascent. In this paper, we introduced a novel detec- References tion method, i.e., language activation probability Josh Achiam, Steven Adler, Sandhini Agarwal, Lama entropy (LAPE), to pinpoint language-specific neu- Ahmad, Ilge Akkaya, Florencia Leoni Aleman, rons within LLMs. LAPE assesses the response Diogo Almeida, Janko Altenschmidt, Sam Altman, of individual neurons to various languages, select- Shyamal Anadkat, et al. 2023. Gpt-4 technical report. ing those with a propensity for activation when ex- arXiv preprint arXiv:2303.08774. posed to one or two languages. Based on LAPE, we Kabir Ahuja, Shanu Kumar, Sandipan Dandapat, and further conducted extensive experiments to inves- Monojit Choudhury. 2022. Multi task learning for tigate the multilingual capabilities of LLMs. 
- **局限/未来工作线索**：rons. For future work, we aim to leverage these preprint arXiv:2305.10403.；Limitations models. URL https://openaipublic. blob. core. win-；limitations of zero-shot language transfer with mul- arXiv preprint arXiv:2002.05202.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Language-Specific Neurons: The Key to Multilingual Capabilities in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
