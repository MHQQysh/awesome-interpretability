# 084. Towards Faithful Natural Language Explanations: A Study Using Activation Patching in Large Language Models

> 逐篇阅读记录：第 84 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Wei Jie Yeo, Ranjan Satapathy, Erik Cambria
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.529)
- **领域标签**：Mechanistic, Attribution, Rationale, Hidden, LLM, Activation, Explain
- **本地 PDF 文本规模**：约 7,734 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Mechanistic为主线，结合论文摘要中的核心设定：Large Language Models (LLMs) are capable of generating persuasive Natural Language Explanations (NLEs) to justify their answers.However, the faithfulness of these explanations should not be readily trusted at face value.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2. We investigate the relationship between the termed as causal effects and can be further decom-；5 Experiments tors where the layers within each token position；7 Limitations Erik Cambria, Lorenzo Malandri, Fabio Mercorio,；1. Disagree: The explanation is illogical or inconsistent with the question and/or does not；2. Neutral: The explanation is somewhat logical and consistent with the question but might；3. Agree: The explanation is logical, consistent with the question, and adequately covers the；1. Disagree: The explanation is unclear or contains overly complex terms or convoluted
- **引言关键线索**：model to assess the level of faithfulness of an ex- The advent of Large Language Models (LLMs) has planation in supporting the explained answer. We revolutionized the field of Natural Language Pro- borrow insights from works that perform Activa- cessing (NLP), enabling machines to perform con- tion Patching (AP), also known as causal tracing siderably well across a wide array of tasks ranging on the LLM internal states (Meng et al., 2022; Vig from commonsense reasoning (Mao et al., 2024) et al., 2020; Zhang and Nanda, 2023), and show to producing high-quality training data (Yeo et al., how the derived causal effects can be used to mea- 2024). sure the faithfulness of a model鈥檚 NLE. 10425 Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing, pages 10425鈥10447 November 4-9, 2025 漏2025 Association for Computational Linguistics Our metric, Causal Faithfulnes
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：should not be readily trusted at face value. parent sophistication, the trustworthiness of these Recent studies have proposed various meth- explanations is not guaranteed and their underlying ods to measure the faithfulness of NLEs, typ- faithfulness (Jie et al., 2024) should be carefully ically by inserting perturbations at the expla- scrutinized. However, it is unclear among the ex- nation or feature level. We argue that these isting approaches (Atanasova et al., 2023; Lanham approaches are neither comprehensive nor cor- et al., 2023; Wiegreffe et al., 2020; Parcalabescu rectly designed according to the established definition of faithfulness. Moreover, we high- and Frank, 2023; Shen et al., 2025), which is best light the risks of grounding faithfulness find- suited to measure the faithfulness. A commonly ref- ings on out-of-distribution samples. In this erenced definition of faithfulness follows as such: work, we leverage a causal med
- **方法与解释性关系**：该论文主要围绕 `Mechanistic, Attribution, Rationale, Hidden, LLM, Activation, Explain` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We compare the consistency, SHAP and For GN, Ca and Ce may, We also include other
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - We compare the consistency between SHAP and For GN, we notice that this form of corruption
  - attribution over magnitude, since Ca and Ce may We also include other baseline faithfulness met-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - improvement over existing faithfulness tests
  - We find that CaF is by design, a closer test of estab- proach, CaF, is similar to CC-SHAP but operates
  - 3. We analyze our approach extensively against to identify features with significant causal effects.
  - biasing features can significantly influence the CoT over a predefined vocabulary set V in an autore-
  - it becomes a counterfactual instance. We find that cal applications (Agarwal et al., 2024). This phe-
  - ple hidden states exhibit significant causal effects.
  - seem less intuitive compared to natural language table limitations. Similar to GN, we show that

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ity of LLM-generated explanations. However, we urge caution in interpreting these findings due to the limited sample size. In this work, we introduced a novel faithfulness metric, Causal Faithfulness, building on insights 5.5 Distribution of Activation Patching from activation patching. We demonstrated that the In addition to measuring the overall divergence be- faithfulness of natural language explanations in a tween the distributions of causal values, we also fo- large language model can be assessed by evaluating cus on analyzing the internal patterns within these the consistency between the causal distributions distributions鈥攕pecifically, the significance of key underlying the explanation and the model鈥檚 answer. input elements such as the prediction itself and Additionally, we expanded on existing critiques corrupted tokens. Our primary focus is on the ag- of current tests, particularly highlighting the risks gregated effects of corrupted tokens, with positions of grounding faithful
- **局限/未来工作线索**：seem less intuitive compared to natural language table limitations. Similar to GN, we show that；are important" but not "how does the model pro- noted limitation of CFF is that it focuses solely on；is explaining. Moreover, when aggregating the ef- nal behavior. Future work could explore integrat-；7 Limitations Erik Cambria, Lorenzo Malandri, Fabio Mercorio,
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Towards Faithful Natural Language Explanations: A Study Using Activation Patching in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
