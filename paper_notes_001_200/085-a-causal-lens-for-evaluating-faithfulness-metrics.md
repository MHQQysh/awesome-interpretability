# 085. A Causal Lens for Evaluating Faithfulness Metrics

> 逐篇阅读记录：第 85 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Kerem Zaman, Shashank Srivastava
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.1496)
- **领域标签**：Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Control
- **本地 PDF 文本规模**：约 15,734 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Large Language Models (LLMs) offer natural language explanations as an alternative to feature attribution methods for model interpretability.However, despite their plausibility, they may not reflect the model's true reas
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Technical Background the reasoning process for input-output pair (x, y)；4 Tasks；6 Conclusion；1. If the explanation contains more than three；2. Otherwise, if it includes a comma followed
- **引言关键线索**：completion, (3) object counting, and (4) multi-hop Natural language explanations from Large Lan- reasoning. Figure 1 illustrates our approach. Us- guage Models (LLMs) have enhanced possibili- ing this benchmark, we conduct the first system- ties for model interpretability, offering readable atic evaluation of prominent faithfulness metrics, insights that surpass traditional feature attribution including Simulatability, corruption-based Chain- methods. Most LLMs can generate explanations of-Thought (CoT) metrics (Lanham et al., 2023), for their predictions at minimal cost (Wei et al., and CC-SHAP (Parcalabescu and Frank, 2023). 2022). However, despite fluency and plausibility, Our findings show that the most diagnostic met- such explanations may not reflect the model鈥檚 ac- ric varies by task and model, but the Filler To- tual reasoning process and can mislead practition- kens metric emerg
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：language explanations as an alternative to fea- ture attribution methods for model interpretabil- themselves. We cannot trust a faithfulness metric if ity. However, despite their plausibility, they we cannot reliably assess whether it actually distin- may not reflect the model鈥檚 true reasoning faith- guishes faithful from unfaithful explanations. Par- fully. While several faithfulness metrics have calabescu and Frank (2023) made initial progress been proposed, they are often evaluated in iso- by comparing different metrics on the same data lation, making principled comparisons between and models, yet their work did not evaluate the them difficult. We present C AUSAL D IAGNOS - effectiveness of the metrics themselves. TICITY , a testbed framework for evaluating We address this critical gap through C AUSAL faithfulness metrics for natural language ex- planations. We use the concept of diagnosticity, D IAGNOSTICITY, an evaluation framework
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：For a baseline faithfulness, Multi-hop Reasoning Task formance, Figure 3, Comparison of original and, Early, ICE, ICE over four tasks, The results indicate that, Table 2, Comparison of diagnosticity scores, CoT corruption-based, CoT- ing which parts, Guozhou Zheng, Huajun Chen. 2024. Figure
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - lation, making principled comparisons between and models, yet their work did not evaluate the
  - For a baseline faithfulness metric that assigns
  - 4.4 Multi-hop Reasoning Task formance, we conduct pairwise comparisons across
  - Figure 3: Comparison of original and modified Early
  - ing (p < 0.05, one-sample t-test) the baseline value fect, even in the repeating setup, indicating both
  - fail to significantly exceed baseline performance.

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1       X, 1          X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and model choice. Our results highlight the planations, ensuring ground truth for evaluation.
  - as (z虃i 鈭 z虃i鈥 ), where a larger drop following cor- conclusions hold across editing paradigms.
  - and the unsigned difference (/z虃i 鈭 z虃i鈥 /) has a significant im- let u and v be explanations (regardless of form, e.g.,
  - 4.3 Object Counting Tasks In this section, we present our results and analyses
  - unchanged. For number questions, we reassign one statistically significant and may vary across mod-
  - entity from the target type and one from other types. els, it significantly outperforms all other metrics
  - scores that are significantly higher than 0.5 (one-sample t-test, p < 0.05).

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ruption indicates a more faithful explanation. For Paraphrasing, we reverse this definition and use 3 Method 1 鈭 (z虃i 鈭 z虃i鈥 ). 2 Our C AUSAL D IAGNOSTICITY framework is in- CC-SHAP Parcalabescu and Frank (2023) assess spired by the idea of diagnosticity, which evaluates faithfulness by aligning input contributions to pre- how well faithfulness metrics distinguish between diction and explanation using SHAP (Lundberg faithful and unfaithful explanations. In 3.1, we sum- and Lee, 2017) scores. They calculate importance marize diagnosticity as introduced by Chan et al. scores for each input token鈥檚 prediction, then for (2022b) for evaluating feature attribution methods. each token in the explanation, aggregating them. In 3.2, we introduce C AUSAL D IAGNOSTICITY, de- Convergence of these score distributions is then scribing how it builds on diagnosticity and extends measured. This method is applicable to both post- it to natural language explanations using causal hoc and Chain-of-Thought (
- **局限/未来工作线索**：limitation is inherent to ICE, we compare ICE over four tasks. The results indicate that synthetic；5.6 Binary vs. Continuous Metrics Another key limitation of current metrics is that；bility, which is a metric that produces binary out- unfaithful. Future work should focus on developing；Limitations tal, Valery Zanimanski, BomSymbols and Twemoji
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“A Causal Lens for Evaluating Faithfulness Metrics”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
