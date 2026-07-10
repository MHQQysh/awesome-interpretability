# 030. Mitigating Hallucinations in Large Vision-Language Models with Instruction Contrastive Decoding

> 逐篇阅读记录：第 30 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xintong Wang, Jingheng Pan, Ding Liang, Chris Biemann
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.937)
- **领域标签**：Evaluation, MLLM, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 8,444 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Large Vision-Language Models (LVLMs) are increasingly adept at generating contextually detailed and coherent responses from visual inputs.However, their application in multimodal decision-making and open-ended generation
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction ally, multimodal misalignment has been identified；4 Experiment；2023. Vicuna: An open-source chatbot impressing；2018 Conference on Empirical Methods in Natural；2024. Perception of knowledge boundary for large
- **引言关键线索**：as a key factor in the occurrence of hallucinations Recent research in large vision-language models (Jiang et al., 2023; Liu et al., 2023a; Wang et al., (LVLMs) (Liu et al., 2023c,b; Li et al., 2023a) has 2023a). To address dataset bias, annotation enrich- seen remarkable progress, benefiting from the inte- ment techniques (Gunjal et al., 2024; You et al., gration of advanced large language models (LLMs) 2023; Zhai et al., 2023) have been introduced. Fur- (Achiam et al., 2023; Touvron et al., 2023a,b) thermore, to counteract the influence of language known for their robust language generation and priors, post-processing strategies (Yin et al., 2023; zero-shot transfer capabilities (Zhong et al., 2023; Zhou et al., 2023) have been developed, along with Peng et al., 2023). In order to leverage off-the- comprehensive initiatives aimed at improving mul- shell LLMs, it is crucial to facilitat
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：proach designed to reduce hallucinations dur- understanding and generative prowess of LLMs, ing LVLM inference. Our method is inspired by the scope of hallucination extends beyond mere our observation that what we call disturbance object existence. It now encompasses more com- instructions significantly exacerbate hallucina- plex elements such as attributes and relationships tions in multimodal fusion modules. ICD con- within the generated content. Consequently, distin- trasts distributions from standard and instruc- guishing discriminative hallucination and the non- tion disturbance, thereby increasing alignment hallucinatory portion in the generation has become uncertainty and effectively subtracting halluci- nated concepts from the original distribution. pivotal in assessing the performance of LVLMs in Through comprehensive experiments on dis- terms of their fidelity to factual visual information. criminative benchmarks (POPE and MME
- **方法与解释性关系**：该论文主要围绕 `Evaluation, MLLM, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：LVLM, LVLM with a posi-, Instruction Contrastive Decoding, Consequently, LVLM Baselines, Leng et al, In comparison to the, VCD approach, ICD, LVLMs and the VCD, VCD both leverage con-, Figure 4 showcases the, Lee, Improved baselines with visual, RLHF, Titanic, Here, LVLMs incor- dation sets
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - scenarios: the baseline LVLM, LVLM with a posi- 3.3 Instruction Contrastive Decoding
  - cilitate this evaluation. Consequently, task scores 4.1.2 LVLM Baselines
  - nally, we compare our method against the visual these issues and object-level hallucinations.
  - contrastive decoding approach (Leng et al., 2023), In comparison to the VCD approach, our ICD
  - language prior to contributing to hallucinations in cantly surpasses the baseline LVLMs and the VCD
  - cline in performance on the position hallucination method and the baseline VCD both leverage con-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - instructions significantly exacerbate hallucina- plex elements such as attributes and relationships
  - demonstrate that ICD significantly mitigates hallucinations in LVLMs. Research efforts have be-
  - hallucinations but also significantly enhances hallucinations, including statistical biases (You
  - stantial human involvement and incur significant 2 Related Work
  - structions can significantly exacerbate hallucina- 2023b) and fine-tuning (Wang et al., 2023c; Wiehe
  - benchmark LLaVa-Bench (Liu et al., 2023c), our tainty in multimodal alignment, significantly con-
  - LLaVA-RLHL (Liu et al., 2023a), seek to improve

## 6. Conclusion、局限与可复现性

- **结论段落线索**：In addressing hallucinations in LVLMs, our ICD cline in performance on the position hallucination method and the baseline VCD both leverage con- task, our method maintains robust performance. trastive decoding tailored for open-ended gener- This distinction underscores the adaptability and ation (Li et al., 2023b). While our ICD method effectiveness of the ICD method in addressing a introduces disturbance instructions to increase mul- broader spectrum of hallucination symptoms, mak- timodal alignment uncertainty, VCD employs dis- ing it a more versatile solution in LVLMs. torted images to amplify visual uncertainty. Posit- Results on MME Benchmark: Our method is ing that a synergistic approach could harness the designed to mitigate hallucinations in LVLMs dur- strengths of both methods, we propose to analyze ing inference. We delve deeper into ascertaining a straightforward integration of these two methods. whether our approach not only preserves but po- tentially enhances the fundamen
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Mitigating Hallucinations in Large Vision-Language Models with Instruction Contrastive Decoding”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
