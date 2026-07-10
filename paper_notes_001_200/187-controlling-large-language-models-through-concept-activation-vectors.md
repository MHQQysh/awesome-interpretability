# 187. Controlling Large Language Models Through Concept Activation Vectors

> 逐篇阅读记录：第 187 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Hanyu Zhang, Xiting Wang, Chengao Li, Xiang Ao, He Qin
- **发表 venue / date**：AAAI / 2025/04
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/34778/36933)
- **领域标签**：Steering, Hidden, LLM, Layer, Control
- **本地 PDF 文本规模**：约 7,863 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Steering为主线，结合论文摘要中的核心设定：As large language models (LLMs) are widely deployed across various domains, the ability to control their generated outputs has become more critical.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2023. More than a Feeling: Accuracy and Application of；2023. Representation engineering: A top-down approach to
- **引言关键线索**：trol (Li et al. 2024). Another approach is parameter fine- Large Language Models (LLMs) (Brown et al. 2020a; tuning (Schulman et al. 2017; Ouyang et al. 2022), which Chowdhery et al. 2023; Touvron et al. 2023) have shown re- demands substantial computational resources and is imprac- markable performance in a variety of tasks, including ques- tical for many users or real-time applications. Fine-tuning tion answering (Shi et al. 2024; Wei et al. 2022a), symbolic can overly specialize the model to a particular dataset, re- reasoning (Hu et al. 2023; Pan et al. 2023), and code gen- ducing its ability to generalize to new contexts and tasks. eration (Roziere et al. 2023). These models are typically Guided decoding is another approach (Dathathri et al. 2020; pre-trained on vast and diverse datasets sourced from the Yang and Klein 2021), which manipulates the probability internet, encompassing 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：quire significant computational resources and extensive trial- varying styles, from formal and precise work documents to and-error or provide coarse-grained control. In this paper, we casual and humorous daily communication. Controlled gen- propose Generation with Concept Activation Vector (GCAV), eration enables AI chatbots to be better adapted for diverse a lightweight model control framework that ensures accu- audiences, ranging from children to sports enthusiasts. rate control without requiring resource-extensive fine-tuning. A common technique for controlled text generation is Specifically, GCAV first trains a concept activation vector for specified concepts to be controlled, such as toxicity. During prompting engineering (Sahoo et al. 2024), which is easy inference, GCAV steers the concept vector in LLMs, for ex- to implement. However, due to the opacity mechanisms ample, by removing the toxicity concept vector from the ac- of LLM
- **方法与解释性关系**：该论文主要围绕 `Steering, Hidden, LLM, Layer, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：These solutions allow us, Baselines We employ Llama-2-7b, Llama-2-7b-, Touvron et al. 2023, We compare, GCAV - Output, Arith- on Twitter data, Antypas et al. 2022a, Formality is evalu-
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - condition is already met. These solutions allow us to com- Baselines We employ Llama-2-7b and Llama-2-7b-
  - pute a specific steering strength for each input prompt. chat (Touvron et al. 2023) as our base model. We compare
  - to the following baselines:
  - put and GCAV - Output, outperforms the baselines in toxic- cific topic when preparing positive and negative prompts for
  - outperforms the other baselines in control success. Arith- on Twitter data (Antypas et al. 2022a,b). Formality is evalu-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - quire significant computational resources and extensive trial- varying styles, from formal and precise work documents to
  - tensive training data enables LLMs to generate human-like mains a significant concern (Zhong et al. 2023).
  - X the-art model in the Llama series. In our results, criterion
  - POSPROMPT 0.5280 2.7428 0.4780 3.6622 our method, GCAV-output, outperforms GCAV-input, likely
  - put and GCAV - Output, outperforms the baselines in toxic- cific topic when preparing positive and negative prompts for
  - outperforms the other baselines in control success. Arith- on Twitter data (Antypas et al. 2022a,b). Formality is evalu-
  - 0.15 0.55 stable, with a slight improvement. This may be because as

## 6. Conclusion、局限与可复现性

- **结论段落线索**：In this paper, we introduce the GCAV framework, a Funds of Renmin University of China No. 24XNKJ18. This lightweight and effective framework for controlled text gen- work was partially done at Beijing Key Laboratory of Big eration in LLMs. Unlike existing approaches that require Data Management and Analysis Methods and Engineering extensive fine-tuning or offer only limited control, GCAV Research Center of Next-Generation Intelligent Search and leverages concept activation vectors to achieve granular ma- Recommendation, Ministry of Education. Xiang Ao was nipulation of specific concepts, such as toxicity, sentiment, also supported by the Project of Youth Innovation Promotion topic, and linguistic style. Experiments across diverse tasks Association CAS, Beijing Nova Program 20230484430, the demonstrate that GCAV effectively controls LLMs outputs Innovation Funding of ICT, CAS under Grant No. E461060. 25857 References Dekoninck, J.; Fischer, M.; Beurer-Kellner, L.; and Vechev, Antypas, D
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Controlling Large Language Models Through Concept Activation Vectors”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
