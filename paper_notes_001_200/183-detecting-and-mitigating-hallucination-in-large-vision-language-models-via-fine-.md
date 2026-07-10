# 183. Detecting and Mitigating Hallucination in Large Vision Language Models via Fine-Grained AI Feedback

> 逐篇阅读记录：第 183 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Wenyi Xiao, Ziwei Huang, Leilei Gan, Wanggui He, Haoyuan Li, Zhelun Yu, Fangxun Shu, Hao Jiang, et al.
- **发表 venue / date**：AAAI / 2025/04
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/34744/36899)
- **领域标签**：Evaluation, MLLM, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 7,414 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：The rapidly developing Large Vision Language Models (LVLMs) still face the hallucination phenomena where the generated responses do not align with the given contexts, significantly restricting the usages of LVLMs.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction primarily categorized into hallucination detection and miti-；2024. MiniGPT-4: Enhancing Vision-Language Under-
- **引言关键线索**：gation. Hallucination detection aims to identify the presence Large Language Models (LLMs) (OpenAI 2023a; Touvron of hallucinations in the LVLM outputs for preventing po- et al. 2023; OpenAI 2023b; Jiang et al. 2023) have marked tential malicious usages (Li et al. 2023b; Wang et al. 2023; a significant milestone in the development of natural lan- Rohrbach et al. 2018; Sun et al. 2023; Liu et al. 2024a; Chen guage processing and have been further extended to en- et al. 2024). Hallucination mitigation aims to enable LVLMs compass multi-modality data, such as language and vi- to generate more faithful responses and can be mainly di- sion, leading to the emergence of Large Vision Language vided into training-free and training-based approaches (Yin Models (LVLMs) (OpenAI 2023c; Liu et al. 2024b; Team et al. 2023b; Huang et al. 2023; Sun et al. 2023; Zhao et al. et al. 2023; Bai et al. 2023). 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：detection on MHaluBench, surpassing GPT-4V and Gemini, lucination, wherein the completions do not align with the and reduces the hallucination rate by 36.1% on AMBER and given contexts. In other words, the generated responses con- 76.3% on Object HalBench compared to the base model. tain incorrect objects, attributes and relations concerning the vision and language inputs, thereby significantly restricting Code 鈥 https://github.com/Mr-Loevan/HSA-DPO the utility of LVLMs (Liu et al. 2024c; Yin et al. 2023a). Research on addressing hallucinations in LVLMs can be
- **方法与解释性关系**：该论文主要围绕 `Evaluation, MLLM, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Figure 1, Comparison of HSA-DPO, No hallucination, None, Our Connection to Scalable, Oversight, Compared with pre-, Claim 79.25 79.02 79.16, DDG, Following Yu et al, For hallucination detection, In- Ours 5.3 3.2
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tection model which can perform sentence-level hallucination Figure 1: Comparison of HSA-DPO (red) with state-of-
  - to <No hallucination, None, 0, None>, respectively. Our Connection to Scalable Oversight. Compared with pre-
  - not being greater considered compared with other hallucina- Claim 79.25 79.02 79.16 79.08
  - lucination in DDG. Following Yu et al. (2024), we use For hallucination detection, we compare our method with
  - itive hallucination mitigation methods as baselines: (1) In- Ours 5.3 3.2 2.1 47.3 13.4 1.2 0.60
  - baselines with visual instruction tuning. arXiv preprint

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1 point, 2 points, 3 points, 1X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - significantly restricting the usages of LVLMs. Most previ-
  - mitigation training. Furthermore, we propose differentiating Notably, HSA-DPO outperforms state-of-the-art models in
  - vision and language inputs, thereby significantly restricting
  - a significant milestone in the development of natural lan- Rohrbach et al. 2018; Sun et al. 2023; Liu et al. 2024a; Chen
  - However, despite the aforementioned efforts, several chal- lucination mitigation, HSA-DPO improves the base LVLM
  - where less significant hallucinations are addressed, while cus on utilizing the abilities of off-the-shelf tools, such as
  - gating LVLM hallucinations in 搂3.2 and 搂3.3, respectively. reasons hji,R and HSji,R improve the explainability of the

## 6. Conclusion、局限与可复现性

- **结论段落线索**：achieves the state-of-the-art results on MHaluBench on av- by alignment tax (Askell et al. 2021; Ouyang et al. 2022) erage, outperforming GPT-4V and Gimini. Specifically, at and maintains its multi-modality capabilities, as evidenced the claim level, our detection model relatively surpasses by the results on the overall metric of MMHal-Bench and UNIHD by 4.7% in Mac. F1 score and GPT-4V Self-Check LLaVA Bench in the wild. Lastly, to investigate the effect 2-shot by 8.1% in Mac. F1 score. This improvement is con- of our method on different base models, we train HSA- sistently observed at the segment level as well. Second, on DPO on Qwen-VL-Chat(Bai et al. 2023), where the hallu- MFHaluBench, our detection model achieves an F1 score of cinated responses are generated by Qwen-VL-Chat. Both 88.2% in binary classification, and an accuracy of 74.3% in DPO and HSA-DPO results in Table 3 indicate that our Multi (fine-grained classification), outperforming GPT-4V model gives a significant impro
- **局限/未来工作线索**：this limitation, we present Hallucination Severity-Aware Di-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Detecting and Mitigating Hallucination in Large Vision Language Models via Fine-Grained AI Feedback”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
