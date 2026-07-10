# 099. Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation

> 逐篇阅读记录：第 99 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Jirui Qi, Gabriele Sarti, Raquel Fernández, Arianna Bisazza
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.347)
- **领域标签**：Attribution, Rationale, Evaluation, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 12,216 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Ensuring the verifiability of model answers is a fundamental challenge for retrieval-augmented generation (RAG) in the question answering (QA) domain.Recently, self-citation prompting was proposed to make large language
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction contextual cues in retrieved sources to evaluate the trust-；9 We further validate the robustness of the CCI fil-；6 Conclusion；2022. Attributed question answering: Evaluation odkumar Prabhakaran, Emily Reif, Nan Du, Ben
- **引言关键线索**：worthiness of models鈥 answers. Retrieval-augmented generation (RAG) with large language models (LLMs) has become the de-facto Ren et al., 2023). However, verifying whether standard methodology for Question Answering the model answer is faithfully supported by the (QA) in both academic (Lewis et al., 2020b; Izac- retrieved sources is often non-trivial due to the ard et al., 2022) and industrial settings (Dao and Le, large context size and the variety of potentially cor- 2023; Ma et al., 2024). This approach was shown rect answers (Krishna et al., 2021; Xu et al., 2023). to be effective at mitigating hallucinations and pro- In light of this issue, several answer attribution2 ducing factually accurate answers (Petroni et al., approaches were recently proposed to ensure the 2020; Lewis et al., 2020a; Borgeaud et al., 2022; trustworthiness of RAG outputs (Rashkin et al., * Equal contribution.
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：using model internals for faithful answer attri- bution in RAG applications. MIRAGE detects context-sensitive answer tokens and pairs them with retrieved documents contributing to their prediction via saliency methods. We evaluate our proposed approach on a multilingual ex- tractive QA dataset, finding high agreement with human answer attribution. On open-ended QA, MIRAGE achieves citation quality and effi- ciency comparable to self-citation while also al- lowing for a finer-grained control of attribution parameters. Our qualitative evaluation high- lights the faithfulness of MIRAGE鈥檚 attributions and underscores the promising application of Figure 1: MIRAGE is a model internals-based answer at- model internals for RAG answer attribution.1 tribution framework for RAG settings. Context-sensitive answer spans (in color) are detected and matched with
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Evaluation, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 2, Agreement, MIRAGE and entailment-based baselines, AA on XOR-AttriQAmatch using, Entailment-based Baselines attribution, MIRAGE in relation to, From the comparison between, Top 3 and Top, ELI5 examples where MIRAGE, Table 6, AA on the full, XOR-AttriQA
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Table 2: Agreement % of MIRAGE and entailment-based baselines with human AA on XOR-AttriQAmatch using
  - 4.2 Entailment-based Baselines attribution, resulting in performances on par with
  - of MIRAGE in relation to self-citation baselines, we for entailment-based answer attribution, we follow
  - From the comparison between Top 3 and Top 5% examine some ELI5 examples where MIRAGE dis-
  - Table 6: Agreement % of MIRAGE and entailment-based baselines with human AA on the full XOR-AttriQA

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：6043
                                                                                                    Percentage
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - the identification of supporting documents (Bohnet butions on two datasets, showing improve-
  - is hindered by the imperfect instruction-following infilled into an LLM prompt to improve the gen-
  - analysis, we find that self-citation often misses rel-
  - dataset (Fan et al., 2019), we find that LLaMA 2 7B
  - tantly, MIRAGE requires extracting model internals Table 2 presents our results. MIRAGE is found to
  - where the answer produced by filling in the con- appears to generally improve MIRAGE鈥檚 agreement
  - significant boost in answer attribution precision

## 6. Conclusion、局限与可复现性

- **结论段落线索**：on ELI5. NLI and MIRAGE produce the correct citation, first step in exploiting interpretability insights to while self-citation does not cite any document ([鈭匽). develop faithful answer attribution methods, paving the way for the usage of LLM-powered question- activation. Thus, despite the high lexical and se- answering systems in mission-critical applications. mantic relatedness, the answer is not supported by Limitations Document [3]. The failure of TRUE in this set- ting highlights the sensitivity of entailment-based LLMs Optimized for Self-citation In this study, systems to surface-level similarity, making them we focus our analysis on models that are not explic- brittle in cases where the model鈥檚 context usage is itly trained to perform self-citation and can provide not straightforward. Using another sampling seed citations only when prompted to do so. While re- for the same query produces the answer 鈥淸...] the cent systems include self-citation in their optimiza- individual can c
- **局限/未来工作线索**：an important limitation for these approaches, since identify which retrieved documents support the；tion results. Future work should produce a wider generate attribution scores in the CCI step can sig-；Applicability to Other Domains and Models paragraph, to future work to further improve MI -；larger models. Future work should extend our anal- The authors have received funding from the
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
