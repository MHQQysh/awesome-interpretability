# 111. Understanding Faithfulness and Reasoning of Large Language Models on Plain Biomedical Summaries

> 逐篇阅读记录：第 111 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Biaoyan Fang, Xiang Dai, Sarvnaz Karimi
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-emnlp.578)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 14,094 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Generating plain biomedical summaries with Large Language Models (LLMs) can enhance the accessibility of biomedical knowledge to the public.However, how faithful the generated summaries are remains an open yet critical q
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Dataset Creation Flan-T5-XL on the PLOS dataset and investi-；5 Supporting Sentence Identification；7 Limitations；6 Conclusions main challenges in benchmarking the faithfulness；2024. Scaling Instruction-Finetuned Language Mod-；2020. BioBERT: a pre-trained biomedical language；I Analysis of Annotated Supporting
- **引言关键线索**：used in domain-specific areas, such as the biomedi- Generating plain text summaries鈥攕ummarising cal domain, remains an open question (Ramprasad technical articles in plain language鈥攈elps facili- et al., 2024). tate public access to biomedical knowledge and has Additionally, current research (Chiang and Lee, been an important topic in the biomedical domain 2023b) has shown that, although LLM-based eval- (Goldsack et al., 2022, 2023; Guo et al., 2021). De- uators achieve promising alignments with human spite the overall performance achieved by LLMs judgment, they do not always provide correct rea- (Jahan et al., 2024; Guo et al., 2024; Sim et al., soning for their decisions, e.g., correct rationales 2023), the faithfulness, which is to what extent the or supporting sentences from text. Examining to generated text is consistent with the source articles, what extent LLMs can provide correct 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：three major research questions: (5) Can LLMs iden- summarisation and measured the faithfulness of tify the supporting sentences from the source article GPT-based text simplification on biomedical ab- when they are instructed to evaluate the faithfulness stracts. of generate summaries? (6) How does the abstrac- Current research has proposed various metrics tiveness of the summary impact the identification based on different frameworks to evaluate the faith- of supporting sentences? (7) Do LLMs perform fulness of generated text for the general domain: better when identifying supporting sentences for (1) QA-based metrics (Scialom et al., 2021; Fab- their own generated summaries? bri et al., 2022; Durmus et al., 2020), utilising QA To the best of our knowledge, our study is the systems to measure the correctness of answering first publicly available benchmark dataset investi- the questions based on the source and summaries, gating faithfuln
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：For the baseline, Okapi BM25, Llama-3 is under licence, LLAMA, Fangyi Yu, Lee Quartey, Frank Schilder. 2023. B, Comparison of the PLOS
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - between current automatic faithfulness evaluators For the baseline, we consider Okapi BM25
  - our models serve as baselines for investigating the terms 18 Llama-3 is under licence 鈥淢ETA LLAMA
  - Fangyi Yu, Lee Quartey, and Frank Schilder. 2023. B Comparison of the PLOS and eLife

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：8.00      percentage
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - We find that generated summaries demonstrate a ing the entailment of the summary (hypothesis)
  - to outperform the GPT family in certain tasks, e.g., lications across various resources. We randomly
  - mostly focuses on general named entities (e.g., per- fulness percentage across selected LLMs. We find
  - pared to traditional metrics. Interestingly, prompt- by other models. In addition, we find that the differ-
  - tences improves the performance of Gemini-1.5 model tendency regarding their faithfulness eval-
  - and Llama-3, but it does not show further improve- uation. For example, when evaluating summaries
  - from high abstractive summaries. Okapi BM25 serve that the model outperforms the others when

## 6. Conclusion、局限与可复现性

- **结论段落线索**：and improve their clinical skills. The VSIP was considered a potential supplement to physical clinical placements and could revolutionize global optometric education by offering co-learning across cultures. [...] The International Eyecare Community (IEC) was created with the purpose to incorporate the inherent advantages of virtual simulation and deliver collaborative global education by offering flexible, diverse, personalised, accessible and equal learning opportunities [4,5]. This platform was not created to replace face-to-face teaching; [...]. Summary It has potential to enhance optometry training by offering flexible, accessible international learning experiences. Extraction Error: Annotator Overlook: #1 The International Eyecare Community (IEC) was created with the purpose to incorporate the inherent advantages of virtual simulation and deliver collaborative global education by offering flexible, diverse, personalised, accessible and equal learning opportunities [4,5] Extraction
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Understanding Faithfulness and Reasoning of Large Language Models on Plain Biomedical Summaries”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
