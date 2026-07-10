# 024. Accelerating Sparse Autoencoder Training via Layer-Wise Transfer Learning in Large Language Models

> 逐篇阅读记录：第 24 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Davide Ghilardi, Federico Belotti, Marco Molinari, Jaehyuk Lim
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.blackboxnlp-1.32)
- **领域标签**：Representation, SAE, Hidden, LLM, Layer, Analyze
- **本地 PDF 文本规模**：约 9,494 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型内部特征叠加、神经元语义不纯导致难以解释的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：Sparse AutoEncoders (SAEs) have gained popularity as a tool for enhancing the interpretability of Large Language Models (LLMs).
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；6 Conclusions to evaluate the transferability of intra-model；2023. Towards automated circuit discovery for mech- Zhang, Simon Vandenhende, Soumya Batra, Spencer；2024. Towards principled evaluations of sparse au- Henryk Michalewski, Ian Tenney, Ivan Grishchenko,；1. Train a SAEi on alternate layers, depending；2. Initialize the current SAEi by either
- **引言关键线索**：Transformer-based models have become ubiqui- tivations using sparse learned features. However, tous in a large variety of different application training SAEs is computationally intensive, par- fields (Dubey et al., 2024; Kirillov et al., 2023; Rad- ticularly when applied across multiple layers in ford et al., 2023; Chen et al., 2021; Zitkovich et al., deep networks. This computational burden poses 2023; Waisberg et al., 2023). Given their tremen- a significant barrier to their widespread applica- dous impact on society, concerns about their inter- tion, especially in resource-constrained environ- pretability have been raised by various stakehold- ments where the cost of training from scratch is ers (Bernardo, 2023). Mechanistic Interpretabil- prohibitive. Recent research has highlighted the po- ity (MI) (Conmy et al., 2023; Nanda et al., 2023), tential of transfer learning as a strategy 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：the quality of learned representations, while right: baseline method where each Sparse AutoEn- significantly accelerating convergence. These coder (SAE) is trained from scratch (solid line); for- findings indicate that the strategic reuse of pre- ward method where SAEs are initialized with weights trained SAEs is a promising approach, particu- from the previous layer鈥檚 SAE and fine-tuned (dashed larly in settings where computational resources line) with the new layer activations; backward method are constrained. where SAEs are initialized with weights from the fol- lowing layer鈥檚 SAE.
- **方法与解释性关系**：该论文主要围绕 `Representation, SAE, Hidden, LLM, Layer, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Sparse AutoEn-, SAEs than the baseline, SAEs, Evaluating ground-truth comparisons, Sharkey et al, Marks et al, Transfer, DLA scores ob-, Baseline training, Train one SAEi, SAE6, SAE5, Normalized, Fwd-SAE and Bwd-SAE respectively
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - right: baseline method where each Sparse AutoEn-
  - it can be seen that forward and backward SAEs than the baseline SAEs, with minimal differences
  - tion, as feature indices are preserved. Evaluating ground-truth comparisons (Sharkey et al., 2023),
  - of comparisons required, and although relevant, it features (Marks et al., 2024) and evaluating SAEs
  - than the baseline, while backward transfer SAEs
  - the 鈥淣o Transfer鈥 baseline, i.e. the DLA scores ob-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：25%, 1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - layers not only maintains but often improves
  - significantly accelerating convergence. These
  - a significant barrier to their widespread applica-
  - 鈥 We show that both Forward-SAEs and tivations into interpretable features (Bricken et al.,
  - scratch, while using significantly less training struction is given by:
  - presents significant challenges. In our work, the on highly activating examples and measured
  - if it improves the CE loss), 鈮 0 when the re- importance, estimated via their Indirect Ef-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：SAEs within models from different families We hypothesized and validated whether SAE trans- that utilize the same architecture. This explo- fer is an effective method to accelerate and opti- ration could offer valuable insights into the mize the SAE training process. We investigated broader applicability of SAEs beyond closely whether SAE weights derived from adjacent layers related model families. could maintain efficacy in reconstruction, which our results affirmed. Furthermore, we examined 鈥 Experimental Scale and Hyperparameter In- whether the transferred SAEs, when fine-tuned on a teractions: Our study was conducted on a lim- layer鈥檚 activations, could reliably capture monose- ited scale in terms of model components in- mantic features comparable to the original SAE, volved and the range of training hyperparame- which has been also confirmed by our experiments. ters explored. The fixed set of hyperparame- The transferred SAEs (both forward and backward) ters used may not fully cap
- **局限/未来工作线索**：7 Limitations and future works ings reveal a 鈥渇eature transfer鈥 phenomenon,；are several limitations that warrant consideration
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Accelerating Sparse Autoencoder Training via Layer-Wise Transfer Learning in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
