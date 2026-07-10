# 135. Leveraging Structured Information for Explainable Multi-hop Question Answering and Reasoning

> 逐篇阅读记录：第 135 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ruosen Li, Xinya Du
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.452)
- **领域标签**：Attribution, Rationale, Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 7,056 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Neural models, including large language models (LLMs), achieve superior performance on multi-hop question-answering.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；5 Further Qualitative Analysis；2023. Decomposed prompting: A modular approach；2011. Identifying relations for open information ex- the 2016 Conference on Empirical Methods in Natu-
- **引言关键线索**：way, which is not a strict 鈥渟ymbolic derivation鈥 Multi-hop question answering (QA) involves an- and often causes inaccurate reasoning (e.g., gener- swering questions that require reasoning over mul- ating answers that don鈥檛 exist or two-hops away). tiple pieces of information, often spread across dif- Instead, we propose a multi-step approach (Fig- ferent parts of a document or multiple documents. ure 1) to tackle this problem: firstly, constructing It looks like 鈥渉opping鈥 over various facts to arrive the semantic graph structures (SG) with informa- at a conclusion or answer. In multi-hop question tion extraction (IE) and then leveraging this sym- answering, the system needs to understand the con- bolic information (including entities and semantic text, maintain the sequence of information, and relations) for strictly guiding the model鈥檚 reasoning utilize this combined information to gen
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：more faithful reasoning chains and substantially multi-hop reasoning. improves the QA performance on two bench- Despite the progress made, several challenges mark datasets. Moreover, the extracted struc- tures themselves naturally provide grounded persist under this paradigm, One is the neural mod- explanations that are preferred by humans, as els鈥 limitations in tackling compositional problems compared to the generated reasoning chains that require strict multi-hop reasoning to derive and saliency-based explanations.1 correct answers (Dziri et al., 2023). CoT-based methods still conduct generations in an end-to-end
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The, After manual analysis and, Askell, Language models are few-shot
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - changes line. The 鈥渢emperature鈥 parameter is set settings. After manual analysis and comparisons
  - bachelor鈥檚 degrees) to make pairwise comparisons Askell, et al. 2020. Language models are few-shot

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - improves the QA performance on two bench-
  - at a conclusion or answer. In multi-hop question tion extraction (IE) and then leveraging this sym-
  - to generate the reasoning chain and answer. The semantic structures help improve question answering.
  - more faithful reasoning chains; (3) We show that In addition, under our prompting-based QA set-
  - to context and help improve the interpretability of the triples, which simplifies the process of encod-
  - significantly improves the reasoning ability of soning. We tackle the task with in-context learning
  - improve the performance of the CoT strategy.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：answering, the system needs to understand the con- bolic information (including entities and semantic text, maintain the sequence of information, and relations) for strictly guiding the model鈥檚 reasoning utilize this combined information to generate a cor- process. rect and comprehensive answer. Traditionally, re- The second challenge is that, although the cur- searchers (Qiu et al., 2019; Tu et al., 2019; Fang rent model-generated reasoning chain provides ex- 1 Code is available at https://github.com/ planations, they are only surface-level interpreta- bcdnlp/Structure-QA. tions, and there is no guarantee that they are fully 6779 Findings of the Association for Computational Linguistics: EMNLP 2023, pages 6779鈥6789 December 6-10, 2023 漏2023 Association for Computational Linguistics Model Input Model Input Documents: Documents: Wikipedia Title: Anne Wikipedia Title: Anne Anne loves ... . She was the daughter of Jean ... Anne loves ... . She was the daughter of Jean ... Information Anne
- **局限/未来工作线索**：explanations that are preferred by humans, as els鈥 limitations in tackling compositional problems；Then the output part would be like below: ber limitation generating the reasoning chain and；Gr盲felfing, Upper Bavaria 鈥 8 June 1991 in length limitation of the LLM. Thus the informa-；Wikipedia Title: Princess Anne of Orl茅ans Limitations
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Leveraging Structured Information for Explainable Multi-hop Question Answering and Reasoning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
