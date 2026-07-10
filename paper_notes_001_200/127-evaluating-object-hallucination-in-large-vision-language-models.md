# 127. Evaluating Object Hallucination in Large Vision-Language Models

> 逐篇阅读记录：第 127 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yifan Li, Yifan Du, Kun Zhou, Jinpeng Wang, Zhao Xin, Ji-Rong Wen
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.20)
- **领域标签**：Evaluation, MLLM, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 9,144 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Inspired by the superior language abilities of large language models (LLM), large vision-language models (LVLM) have been recently proposed by integrating powerful LLMs for improving the performance on complex multimodal
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 Background form complex reasoning over the related concepts；600 MiniGPT-4；500 MultiModal-GPT；60 MiniGPT-4；50 MultiModal-GPT；5 POPE；6 Conclusion
- **引言关键线索**：lucination would degrade the model performance Large language models (LLMs) (Zhao et al., 2023) and greatly harm the user experiences in real-world have shown remarkable abilities to solve various applications (MacLeod et al., 2017; Ji et al., 2022). complex tasks by following human instructions in Therefore, it is natural to ask the question: does a zero-shot manner. The success of LLMs drives hallucination still exist in LVLMs? In this paper, we the researchers to devise more powerful multi- systematically evaluate the issue of object halluci- modal models based on the superior capacity of nation in existing LVLMs, which refers to generat- LLMs, to enhance the understanding of visual se- ing contents that are inconsistent with ground-truth mantics (Alayrac et al., 2022; Li et al., 2023b). As objects in the given image. an exemplified work, GPT-4 (OpenAI, 2023) has To conduct our study,
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：4 Meituan Group {liyifan0925, yifandu1999, batmanfly}@gmail.com, francis_kun_zhou@163.com, wangjinpeng04@meituan.com, jrwen@ruc.edu.cn Abstract 2023a) have been proposed to enhance the vision- language pre-trained model (VLPM) (Gan et al., Inspired by the superior language abilities of 2022) by incorporating powerful LLMs (Touvron large language models (LLM), large vision- et al., 2023; Chiang et al., 2023), which are called language models (LVLM) have been recently proposed by integrating powerful LLMs for large vision-language model (LVLM). Typically, ex- improving the performance on complex multi- isting work reuses the visual encoder in VLPMs to modal tasks. Despite the promising progress handle image data, while replacing the original lan- on LVLMs, we find that they suffer from ob- guage encoder with LLMs. After vision-language ject hallucinations, i.e., they tend to generate pre-training (Alayrac et al., 2022; Li et al., 2022b) o
- **方法与解释性关系**：该论文主要围绕 `Evaluation, MLLM, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：CHAIRI and CHAIRS, As a comparison, CHAIR metrics compared with, Section 4.1 and, Table 7, Comparison of the evaluated, LVLMs, LLM stand for Visual, Encoder, Alignment Network and
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - variants, i.e., CHAIRI and CHAIRS , which evalu- deserves to be concerned about. As a comparison,
  - bled values of CHAIR metrics compared with those first make two basic hypotheses in Section 4.1 and
  - struction data. A detailed comparison (e.g., back-
  - (2023b) as baseline results.
  - Table 7: Comparison of the evaluated LVLMs. VE, AN, LLM stand for Visual Encoder, Alignment Network and

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - on LVLMs, we find that they suffer from ob- guage encoder with LLMs. After vision-language
  - object hallucination. Experiment results show text (Huang et al., 2021; Bang et al., 2023), and
  - small vision-language models. Besides, we find LLMs as Large Vision-Language Models (LVLM).
  - about these objects, thus achieving an improved
  - really contain 鈥渄ining table鈥. We show the re- the hallucinated objects in each image belong to the
  - ter support our results, we also list the full statistics pling strategies in our evaluation pipeline.
  - we can obtain a similar conclusion as in Table 1 ple closed-ended questions, which is less likely to

## 6. Conclusion、局限与可复现性

- **结论段落线索**：that InstructBLIP performs the best, while LLaVA, introduce ambiguity compared to instruction-based MultiModal-GPT and mPLUG-Owl suffer more se- methods. Such characteristic contributes to its sta- vere hallucination problem, whose F1 Score are be- bility. To validate it, we evaluate LLaVA using both low 70. It indicates that POPE can well estimate the POPE and CHAIRI with four different prompts for degree of the hallucination problem in LVLMs. Be- each. The evaluation results are presented in Ta- sides, we find that LLaVA, MultiModal-GPT and ble 4. It can be observed that the standard deviation 298 POPE CHAIR Prompt F1 Score Prompt CHAIRI Is there a <object> in the image? 68.65 Generate a short caption of the image. 10.50 Does the image contain a <object>? 66.83 Provide a brief description of the image. 18.80 Have you noticed a <object> in the image? 66.67 Generate a concise description for the image. 14.60 Can you see a <object> in the image? 67.58 Create a short textual summary for 
- **局限/未来工作线索**：lem in LVLMs and highlighted the limitations of per image. oi can be obtained either from annota-；OKVQA and Accuracy on GQA. For POPE, we copy 7 Limitations；image captioning tasks. For VQA tasks, we eval- this work still has several limitations. First, we；in POPE with traditional metrics. The evaluation Second, due to the limitation of computation re-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Evaluating Object Hallucination in Large Vision-Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
