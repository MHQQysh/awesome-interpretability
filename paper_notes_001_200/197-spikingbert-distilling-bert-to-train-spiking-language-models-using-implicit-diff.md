# 197. SpikingBERT: Distilling BERT to Train Spiking Language Models Using Implicit Differentiation

> 逐篇阅读记录：第 197 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Malyaban Bal, Abhronil Sengupta
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/28975/29854)
- **领域标签**：Attribution, Evaluation, Hidden, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 7,908 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Large language Models (LLMs), though growing exceedingly powerful, comprises of orders of magnitude less neurons and synapses than the human brain.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2023. Constructing deep spiking neural networks from arti-
- **引言关键线索**：ated time steps (like in BPTT), we leverage the convergence Large language Models (LLMs) are becoming increasingly of the average spiking rate (ASR) of the neurons to an equi- popular because of its broad applications in a variety of librium state during the 鈥渇orward鈥 phase of learning. Upon natural language processing (NLP) tasks. LLMs like GPT- convergence, we can derive fixed-point equations from the 3 (Brown et al. 2020) has shown additional characteristics underlying model and subsequently employ implicit differ- such as emergent abilities (Wei et al. 2022) which can only entiation on the attained steady-state to train the model pa- be realized once the model size/compute increases above a rameters effectively as described later on. certain threshold. Recent times have witnessed commercial Training using implicit differentiation is primarily used deployment of LLMs enabling worldwid
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：one to demonstrate the performance of an operational spik- training spiking LMs. We consider our spiking LM as a dy- ing LM architecture on multiple different tasks in the GLUE namical system that, given an input, progressively converges benchmark. Our implementation source code is available at to a steady-state (over Tconv time steps). Similar to most su- https://github.com/NeuroCompLab-psu/SpikingBERT. pervised learning algorithms, training is done in two phases, viz. 鈥渇orward鈥 and 鈥渂ackward鈥. However, instead of learn- Introduction ing through unrolling the computational graph over the oper- ated time steps (like in BPTT), we leverage the convergence Large language Models (LLMs) are becoming increasingly of the average spiking rate (ASR) of the neurons to an equi- popular because of its broad applications in a variety of librium state during the 鈥渇orward鈥 phase of learning. Upon natural language processing (NLP) tasks. LLMs like GPT-
- **方法与解释性关系**：该论文主要围绕 `Attribution, Evaluation, Hidden, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Because appropriate neuromorphic baselines, BPTT, Baselines, SpikingBERT Settings, LM and evaluate it, Our goal for the
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - librium approximates vanilla non-spiking attention. Because appropriate neuromorphic baselines are currently
  - unavailable, we compare our proposed model with existing
  - this approach in comparison to BPTT.
  - formance considerably when compared with no-feedback
  - for classification tasks can be written as, Baselines & SpikingBERT Settings
  - posed spiking LM and evaluate it against different tasks in are non-spiking architectures. Our goal for the comparisons

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significantly more power/energy to operate. In this work, we creating a bio-plausible and energy-efficient solution.
  - romorphic hardware, thereby resulting in significant energy
  - Training LMs from scratch is a significant time and to the best of our knowledge, that has been evaluated against
  - Moreover, the feasibility of model training during distilla- significant attention. TinyBERT (Jiao et al. 2019) proposed
  - Unlike in vision-based tasks, feedback did not improve per-
  - ations - which has been shown to significantly reduce com- Lhi = M SE(a鈭梙i WTd , Tf (hi ) ) (10)
  - Table 1: Results showing performance of our model (SpikingBERT4 ) against some standard models and other efficient imple-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Analysis of Power & Energy Efficiency Drawing inspiration from the astonishing intricacy of the human brain, a complexity that outshines that of any cur- The proposed spiking LM consists of less number of param- rent LLM, we have the opportunity to leverage these in- eters than the 鈥渢eacher鈥 BERT models (109M) and in addi- sights in crafting models that not only replicate biologically tion to that it uses only accumulative (ACC) operations in plausible behavior but also offer energy-efficient solutions place of multiplicative and accumulative operations (MAC) through minimal power consumption. In this paper, we pro- found in vanilla BERT models. Considering 45nm CMOS pose a spiking LM and evaluate it against multiple tasks technology, ACC operations exhibit an impressive energy in the GLUE benchmark. Leveraging steady-state conver- efficiency, consuming only 0.9pJ, which is over five times gence, we introduced a spiking attention mechanism, pro- (5.1) more efficient than MAC operations
- **局限/未来工作线索**：drop in accuracy across all datasets. Conclusion and Future Works
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“SpikingBERT: Distilling BERT to Train Spiking Language Models Using Implicit Differentiation”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
