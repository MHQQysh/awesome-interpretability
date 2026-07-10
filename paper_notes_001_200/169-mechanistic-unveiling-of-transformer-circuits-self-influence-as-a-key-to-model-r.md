# 169. Mechanistic Unveiling of Transformer Circuits: Self-Influence as a Key to Model Reasoning

> 逐篇阅读记录：第 169 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Lin Zhang, Li Hu, Di Wang
- **发表 venue / date**：NAACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-naacl.76)
- **领域标签**：Mechanistic, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 13,516 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Mechanistic为主线，结合论文摘要中的核心设定：Transformer-based language models have achieved significant success; however, their internal mechanisms remain largely opaque due to the complexity of non-linear interactions and high-dimensional operations.While previou
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Related Work；4 Method；2022. Selection-inference: Exploiting large language Mengdi Li, Pan Zhou, Muhammad Asif Ali, and；2023. Datainf: Efficiently estimating data influence；2020. Explaining black box predictions and unveil-；2024. Have faith in faithfulness: Going beyond cir-
- **引言关键线索**：stantial memory consumption required. In recent years, the Transformer architecture, in- As a result, circuit analysis within the frame- troduced by (Vaswani et al., 2017), has become work of Mechanistic Interpretability (MI) (Olah, an efficient neural network structure for sequence 2022; Nanda, 2023) has become a focal point of modeling (Brown et al., 2020). Previous research research. MI aims to discover, understand, and (Hou et al., 2023; Dong et al., 2021) has confirmed verify the algorithms encoded in model weights by that large models rely primarily on reasoning rather reverse engineering the model鈥檚 computations into than mere memorization when answering questions. human-understandable components (Meng et al., However, the "thought process" of these models re- 2022; Geiger et al., 2021; Geva et al., 2020; Zhang mains unclear, as shown in Figure 1. How can et al., 2024; Hong et al.
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：diction task (IOI) and demonstrate that the un- map the thought process executed by the model. derlying circuits reveal a human-interpretable However, directly calculating self-influence for all reasoning process used by the model. parameters in LLMs is practically infeasible due to the enormous computational resources and sub-
- **方法与解释性关系**：该论文主要围绕 `Mechanistic, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Baselines, As SICAF is a, Figure 2, Comparison of normalized faithfulness, Table 4, Comparison of EAP, EAP-IG, EAP-IG-KL methods. Each method
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - to x is computationally efficient in deep learning and corrupted inputs for direct comparison, testing
  - = (I 鈭 H)i (9) Baselines. As SICAF is a mechanistic interpreta-
  - Figure 2: Comparison of normalized faithfulness, number of nodes, and parameter percentage for circuits identified
  - ically, our baseline includes the following methods:
  - A.3 Baselines "greedy algorithm":
  - Table 4: Comparison of EAP, EAP-IG, and EAP-IG-KL methods. Each method differs in at least one aspect in

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X, 1X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - achieved significant success; however, their in- et al., 2022; Chen et al., 2023; Hu et al., 2024a;
  - fication (IOI) (Wang et al., 2022). We find that Interpretability Methods in Language Models.
  - model, significantly reducing the computational
  - proposed to enhance computational efficiency. Be- Leibler (KL) divergence as the loss improves upon
  - on a test sample zt is: EAP-IG improves fidelity by more accurately cap-
  - (Hanna et al., 2024), which combines EAP-IG with EAP-IG-KL outperform EAP in terms of normal-
  - model layers varies significantly between methods.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：information. This balanced distribution aligns well with tasks requiring multi-step reasoning, as it sup- We propose a new mechanistic interpretation frame- ports the retention and transformation of informa- work, SICAF, to trace and analyze the thought pro- tion throughout the model鈥檚 layered structure. EAP- cesses that language models (LMs) employ dur- IG-KL, in particular, achieves a high level of bal- ing complex reasoning tasks, and we validate our ance, ensuring stable self-influence scores across approach on the GPT-2 model in the IOI reason- layers. This feature suggests that EAP-IG-KL is ing task. By applying circuit analysis and self- not only more robust in handling complex reason- influence functions, we successfully mapped the ing tasks but also better equipped to leverage infor- reasoning pathways within the GPT-2 model dur- mation from both lower and higher layers. ing the IOI task. Our method reveals a hierarchical The common hierarchical reasoning path fol- structure i
- **局限/未来工作线索**：To address the aforementioned limitations, we insights into the challenges of mechanistic inter-；some limitations. First, the analysis was conducted standing the origins of bias in word embeddings. In
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Mechanistic Unveiling of Transformer Circuits: Self-Influence as a Key to Model Reasoning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
