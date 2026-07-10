# 198. T-SciQ: Teaching Multimodal Chain-of-Thought Reasoning via Large Language Model Signals for Science Question Answering

> 逐篇阅读记录：第 198 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Lei Wang, Yi Hu, Jiabang He, Xing Xu, Ning Liu, Hui Liu, Heng Tao Shen
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/29884/31543)
- **领域标签**：Rationale, Evaluation, MLLM, Reasoning, Behavior, Evaluate
- **本地 PDF 文本规模**：约 8,137 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Large Language Models (LLMs) have recently demonstrated exceptional performance in various Natural Language Processing (NLP) tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：Introduction / Method / Experiments / Conclusion
- **引言关键线索**：Scientific problem solving has recently been employed to evaluate the multi-hop reasoning capability and interpretabil- multimodal inputs and incorporate external knowledge to ity of AI systems (Kembhavi et al. 2017; Sampat, Yang, and answer scientific questions. Baral 2020; Dalvi et al. 2021). However, these datasets (Kem- Recently, Large Language Models (LLMs) have shown bhavi et al. 2017; Jansen et al. 2018) suffer from limited scale. exceptional performance in various Natural Language Pro- To address this issue, Lu et al. (2022a) introduces a large- cessing (NLP) tasks (Brown et al. 2020; Thoppilan et al. scale science question-answering dataset across broad topics 2022). Specifically, they have demonstrated the chain-of- and skills called ScienceQA. This dataset consists of 21,208 thought (CoT) ability to solve complex reasoning problems multimodal data examples associated with ques
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：aims at teaching science question answering with LLM signals. Generated CoT Signal The T-SciQ approach generates high-quality CoT rationales as Lecture: N/A teaching signals and is advanced to train much smaller models Solution: West Virginia is the farthest north of the four states listed. West Virginia is located in the Appalachian region of the United States, to perform CoT reasoning in complex modalities. Addition- which is in the northeastern part of the country. Louisiana, Arizona, and ally, we introduce a novel data mixing strategy to produce Oklahoma are all located in the southern and southwestern parts of the United States. West Virginia is the northernmost of the four states, more effective teaching data samples for simple and complex making it the farthest north. science question answer problems. Extensive experimental results show that our T-SciQ method achieves a new state- of-the-art performance on the ScienceQA benchmark
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, MLLM, Reasoning, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The code is question, Annotations in-, Our teaching follows the, Multimodal-CoT, Zhang et al. Baselines, We provide a comparison, Specifically, DFAF, Gao et al. 2019, These VQA baselines use, LLM-based multimodal fine-tuned baselines, Ti to the original, The new also include, CoT baselines over different, API-based OpenAI, Mutimodal-CoTLarge, Mutimodal-CoTLarge 91.68 modal baseline, LLaVa
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - the most powerful fine-tuned baseline by 4.5%. The code is question, context, image, skill, and options. Annotations in-
  - Our teaching follows the Multimodal-CoT (Zhang et al. Baselines. We provide a comparison of our proposed method
  - 2023b) two-stage fine-tuning framework: rationale gener- with extensive baseline methods. Specifically, we have sev-
  - nale generation model Fr (Pi ) is trained to predict the teach- 2018), DFAF (Gao et al. 2019). These VQA baselines use
  - where 胃r represents learnable parameters of the rationale recent LLM-based multimodal fine-tuned baselines such
  - rationale Ti to the original language input Xi,la . The new also include widely-used in-context learning baselines: the

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - results show that our T-SciQ method achieves a new state-
  - an accuracy of 96.18%. Moreover, our approach outperforms Figure 1. The input of a ScienceQA data example includes a
  - eration models in scientific problems may result in significant ing by prompting large language models to generate interme-
  - work called Multimodal-CoT that models both language and improve CoT prompting from different aspects, including
  - The Multimodal-CoT method has a significant disadvan- 2022b; He et al. 2023) and improving the quality of reason-
  - rationale for a QA data example to obtain a QA-CoT sample. Distilling step-by-step (Hsieh et al. 2023) improves small
  - to obtain QA-PCoT teaching samples. method highly improves student performance in complex

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Multimodal-CoT on the ScienceQA benchmark (Figure 5). Figure 5a shows cases needing geographic knowledge. This paper introduces a new approach named T-SciQ that Human-annotated CoT may lack open-world information, utilizes large language models鈥 chain-of-thought (CoT) rea- while T-SciQ includes it. Figure 5b shows a multi-step rea- soning capabilities to teach small multimodal models for soning case without image input. Multimodal-CoT errors complex science question answering tasks. Our zero-shot while our model decomposes and answers correctly. These prompting method generates QA-CoT samples as teaching highlight that T-SciQ is well-suited to handle problems that data. We also present a 3-step zero-shot prompting approach require open knowledge and decomposition. using plan-based CoT for highly complex problems. Fur- thermore, our data mixture strategy combines CoT and plan- Comparison on Other NLP Reasoning Datasets. To ver- based CoT to create a new T-SciQ teaching dataset. Our ify 
- **局限/未来工作线索**：reasoning ability, it has two fundamental limitations. First, the by appending a prompt like 鈥淟et鈥檚 think step by step鈥 to；elicit CoT reasoning ability, it has two inherent limitations: eral and brief plan step by step to solve these problems (begin；ify the versatility of our teaching approach, we additionally method overcomes the limitations of human-annotated CoT,；Teacher (Ho, Schmid, and Yun 2022): arithmetic (Aqua (Ling answering. Future work includes exploring extensive LLMs
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“T-SciQ: Teaching Multimodal Chain-of-Thought Reasoning via Large Language Model Signals for Science Question Answering”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
