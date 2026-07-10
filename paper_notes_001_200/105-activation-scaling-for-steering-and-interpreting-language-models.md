# 105. Activation Scaling for Steering and Interpreting Language Models

> 逐篇阅读记录：第 105 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Niklas Stoehr, Kevin Du, Vésteinn Snæbjarnarson, Robert West, Ryan Cotterell, Aaron Schein
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-emnlp.479)
- **领域标签**：Attribution, Rationale, Steering, Hidden, LLM, Activation, Control
- **本地 PDF 文本规模**：约 8,463 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Given the prompt "Rome is in", can we steer a language model to flip its prediction of an incorrect token "France" to a correct token "Italy" by only multiplying a few relevant activation vectors with scalars?We argue th
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；4 Experiments ful according to our effectiveness objective illustrated；5 Extension to Variable-Length Prompts；10 Figure 8: We compare ACTIV S CALAR on GPT2-Small；7 Conclusion studied and the small size and synthetic character；2024. Inspecting and editing knowledge represen-；2024. Competition of mechanisms: Tracing how
- **引言关键线索**：as interventions to other model locations may Understanding which components of a language be similarly effective. Indeed, recent work has model play which roles in which tasks is a core aim questioned the relationship between interpretability of mechanistic interpretability. Given the prompt and intervention on this basis, and has advocated Rome is in, for instance, one might ask which com- for more rigorous and deliberate methodology for ponents of the model most influence it to favor Italy connecting the two (Hase et al., 2023; Wang and over some incorrect answer token, such as France. Veitch, 2024; Hanna et al., 2024). In addressing this question, a natural axiom is that A parallel literature on model steerability also a given component can only be understood as influ- seeks to develop effective interventions, not for ential for a given task if intervening on it meaning- the primary 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：much more minimal allowing us to pinpoint in- 2019; Vig et al., 2020; Meng et al., 2022), among terpretable model components. We evaluate ac- others. Studies in this literature often produce a tivation scaling from different angles, compare set of attribution scores associated with various performance on different datasets, and make locations in the model which represent how much activation scalars a learnable function of the activation vectors themselves to generalize to the model鈥檚 output changed after editing the varying-length prompts.1 activation vectors at each location. Although an effective intervention may be necessary to believe a given localization hypothesis, it is not sufficient,
- **方法与解释性关系**：该论文主要围绕 `Attribution, Rationale, Steering, Hidden, LLM, Activation, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We compare ACTIV S, CALAR and S TEERV, EC for, Figure 8, CALAR on GPT2-Small
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - and minimality (right) on train (crosses) and test sets (points). We compare ACTIV S CALAR and S TEERV EC for
  - 10 Figure 8: We compare ACTIV S CALAR on GPT2-Small

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - intervening on a model is a prerequisite for Figure 1: We show that it is often sufficient to scale
  - necessarily localized) interventions to the weights ple tasks. Moreover, we find activation scalars are
  - suite of experiments, we find overall that activation Most state-of-the-art language models rely on
  - fewer learnable parameters. Our results suggest ers of Transformer blocks, each of which consists
  - and mlpOut on all layers and token positions of GPT2-Small. We find that ACTIV S CALAR does not fall behind
  - Effectiveness versus Minimality. We find that
  - Effectiveness versus Faithfulness. We find that minimal qualitatively, as it is limited to affecting

## 6. Conclusion、局限与可复现性

- **结论段落线索**：quires thresholding the attribution scores associ- is supported by the Swiss Data Science Center ated with model components and then zeroing out (SDSC) PhD fellowship. V茅steinn Sn忙bjarnarson (De Cao et al., 2022; Wang et al., 2023; Conmy is funded by the Pioneer Centre for AI, DNRF grant et al., 2023; Syed et al., 2023) or corrupting them number P1. (Geiger et al., 2021; Bhaskar et al., 2024). However, this means that the intervention can be considered Limitations discrete, as it cannot smoothly facilitate different intervention strengths and directions. The scaling S TEERV EC, ACTIV S CALAR and DYN S CALAR are aspect of the 尾 hyperparameters in ACTIV S CALAR controllable via different hyperparameters: the mar- and S TEERV EC, in turn, is continuous. gin m in the effectiveness objective, 位F weigh- ing the faithfulness term and 位M weighing the Gradient-Based Steering and Interpretability. strength of the 鈩1 -regularization. There are addi- This work relies on gradient-based optimization
- **局限/未来工作线索**：this means that the intervention can be considered Limitations；A limitation of this work is the size of models
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Activation Scaling for Steering and Interpreting Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
