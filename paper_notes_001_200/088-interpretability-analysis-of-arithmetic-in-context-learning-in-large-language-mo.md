# 088. Interpretability Analysis of Arithmetic In-Context Learning in Large Language Models

> 逐篇阅读记录：第 88 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Gregory Polyakov, Christian Hepting, Carsten Eickhoff, Seyed Ali Bahrainian
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.92)
- **领域标签**：Analysis, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 12,471 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Analysis为主线，通过表征分析、因果干预或解释评估来连接内部机制与外部行为。
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction ken, where it modulates higher-level computations；2 Related Work；3 Methodology mt or at at the chosen position t and layer k,；4 Experimental Results；2023. Pythia: A suite for analyzing large language Dahle, Aiesha Letman, Akhil Mathur, Alan Schelten,；2017. Linking news across multiple streams for time-；X The experiments 1-7 in Section 4 consistently high-；1. Given a set of prompts S (all containing ICEs),
- **引言关键线索**：by interacting with task-specific information. Numerical reasoning in natural language tasks, We focus on two key aspects of the ICE: (1) such as solving arithmetic problems, poses a signif- symbol-level information, encompassing individ- icant challenge for large language models (LLMs) ual tokens, operands, operators, and the arithmetic (Testolin, 2024). Despite excelling in diverse do- correctness of the ICE; and (2) pattern-level infor- mains, LLMs struggle with text-based mathemati- mation, which includes consistency in formatting cal problems (Welleck et al., 2022). (e.g., 鈥6鈥 vs. 鈥渟ix鈥) and the arrangement of tokens A promising approach for improving LLM per- within the equation. Via a comprehensive set of formance in this area involves the use of in-context counterfactual activation patching experiments, we examples (ICEs), where a model is given one examine how corrupting these d
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：formance in this area involves the use of in-context counterfactual activation patching experiments, we examples (ICEs), where a model is given one examine how corrupting these distinct symbol- and or more task demonstration prior to the main pattern-level structures within an ICE impacts the task. This method has shown considerable promise model鈥檚 internal processing and task accuracy. We 1 https://github.com/ali-bahrainian/arithmetic-icl- find that pattern-level information is more crucial interpretability for guiding the model towards correct arithmetic 1758 Proceedings of the 2025 Conference on Empirical Methods in Natural Language Processing, pages 1758鈥1777 November 4-9, 2025 漏2025 Association for Computational Linguistics Experiment Description Category Accuracy Exp. 1: Information flow analysis Analysis of the activation patterns in the residual stream at ICE and task Baseline 100% tokens. Exp. 2: Activation patching analysis Id
- **方法与解释性关系**：该论文主要围绕 `Analysis, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Exp, Information flow analysis Analysis, ICE and task Baseline, Activation patching analysis Identification, MLP and attention modules, Baseline 100, Brown et al, Henighan et al, Llama-3, Figure 3b, Step 2, Appendix C, This step measures, Future work can
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Exp. 1: Information flow analysis Analysis of the activation patterns in the residual stream at ICE and task Baseline 100%
  - Exp. 2: Activation patching analysis Identification of MLP and attention modules鈥 contribution to arithmetic Baseline 100%
  - reasoning (Brown et al., 2020; Henighan et al., a 30.0% zero-shot (p0 ) baseline to 76.7% with a
  - this baseline understanding, we conducted coun- while the core mechanism is similar, Llama-3.1-8B
  - impact on the model鈥檚 performance. baseline zero-shot accuracy was 48.6%, which in-
  - non-numeral symbols (e.g., 鈥1+3+4=8鈥 to 鈥渁l- the baseline accuracy improved from 30.0% (zero-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - tially improve performance on zero-shot arithmetic mation flow routes (Ferrando and Voita, 2024), ana-
  - full ICEs. This reinforces our conclusion that mod- network pathways. MI has been applied to under-
  - ing their internal components (Elhage et al., 2021; essence, which can improve performance on simple
  - grades with increased calculation complexity and dataset, Llama-3.1-8B鈥檚 accuracy improved from
  - substantially improved accuracy to 52.1% from To complement our activation patching experi-
  - further improvements. formation transfer, we also employ Information
  - the ICE result token, transferring this information ICE鈥檚 result. Moreover, significantly high PEs oc-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：els primarily rely on abstract pattern-level cues of stand factual recall (Meng et al., 2023), linguistic arithmetic ICEs rather than exact symbolic content. phenomena (Wang et al., 2022), and simpler two- Our main contributions are: (1) We present operand arithmetic tasks (Stolfo et al., 2023). Our the first mechanistic interpretability study focused work employs a suite of MI techniques, such as on how LLMs process ICEs for complex, three- activation patching, information flow analysis and operand arithmetic tasks. (2) We identify and attribution patching, to investigate complex three- validate the core computational pathway through operand arithmetic with in-context examples. which ICE information influences task outputs. (3) In-Context Learning (ICL). ICL enables LLMs We demonstrate that pattern-level consistency in to perform tasks with minimal examples, bypass- ICE formatting plays a more critical role than sym- ing model fine-tuning (Brown et al., 2020; Liu bolic correctness or 
- **局限/未来工作线索**：crucial for encoding the result token. Although 6 Conclusions and Future Work；racy, in comparison the effect is less pronounced. on compute-intensive methods. Future work can；G Compatibility Limitations of the MPT-7B, and LLama-3.1-8B. Llama-3.1-8B also
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Interpretability Analysis of Arithmetic In-Context Learning in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
