# 136. Towards Interpretable Mental Health Analysis with Large Language Models

> 逐篇阅读记录：第 136 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Kailai Yang, Shaoxiong Ji, Tianlin Zhang, Qianqian Xie, Ziyan Kuang, Sophia Ananiadou
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.370)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 15,320 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：The latest large language models (LLMs) such as ChatGPT, exhibit strong capabilities in automated mental health analysis.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction mostly use simple prompts to detect mental health；1) We evaluate four representative LLMs on mental Mental Health Analyzer；4 Results and Analysis；3 Experimental Settings We conduct all LLaMA experiments on a single；7513. Wenxiang Jiao, Wenxuan Wang, Jen-tse Huang, Xing；2022. Training language models to follow instruc-；2023. Chatgpt as a factual inconsistency evaluator The 3rd Social Media Mining for Health Applications；3021. Shichun Liu, Yuhan Cui, Zeyang Zhou, Chao Gong,
- **引言关键线索**：WARNING: This paper contains examples and de- conditions directly. These vanilla methods ig- scriptions that are depressive in nature. nore useful information, especially emotional cues, Mental health conditions such as depression which are widely utilized for mental health analysis and suicidal ideation seriously challenge global in previous works (Zhang et al., 2023). We believe 鈭 it requires a comprehensive exploration and evalu- Equal contribution, listed alphabetically. 鈥 Corresponding author. Qianqian is now affiliated with ation of the ability and explainability of LLMs on Yale University. The work was done when she was at The mental health analysis, including mental health de- University of Manchester. 1 tection, emotional reasoning, and cause detection The data is released at https://github.com/ 2 SteveKGYang/MentalLLaMA https://openai.com/blog/chatgpt 6056 Proceedings of the 20
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：leading to a novel dataset with 163 human- assessed explanations1 . We benchmark existing Though previous works depict a promising fu- automatic evaluation metrics on this dataset to ture for a new LLM-based paradigm in mental guide future related works. According to the health analysis, several issues remain unresolved. results, ChatGPT shows strong in-context learn- Firstly, mental health condition detection is a safe- ing ability but still has a significant gap with ad- critical task requiring careful evaluation and high vanced task-specific methods. Careful prompt transparency for any predictions (Zhang et al., engineering with emotional cues and expert- 2022a), while these works simply tested on a few written few-shot examples can also effectively binary mental health condition detection tasks and improve performance on mental health analysis. In addition, ChatGPT generates explanations lack the explainability on detection results.
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：OpenAI API. Each prompt, T-SID, Ji et al, For cause, Zero-shot Prompting. In the, Baseline Models. We select, LLaMA-13BZS achieves, BERT, MentalRoBERTa, Details ability. Compared with, Appendix D.2. ChatGPTZS significantly, Therefore, We notice that T-SID, The results of baseline, RoBERTa-based and word Baseline, Models We compare the
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - comparisons. 42 posts that are incorrectly classi-
  - the benchmark datasets, baseline models, and au- via the OpenAI API. Each prompt is fed indepen-
  - dataset T-SID (Ji et al., 2022a). For cause/factor Zero-shot Prompting. In the comparison
  - Baseline Models. We select the following base- expanded model size, LLaMA-13BZS achieves
  - BERT/MentalRoBERTa (Ji et al., 2022b). Details ability. Compared with supervised methods,
  - about these baseline models are in Appendix D.2. ChatGPTZS significantly outperforms traditional

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - ing ability but still has a significant gap with ad- critical task requiring careful evaluation and high
  - improve performance on mental health analysis.
  - We evaluate four LLMs with varying model sizes though it still significantly underperforms advanced
  - et al., 2022), emotion-enhanced prompting, and written examples also significantly improves model
  - 2023) is a set of open-source LLMs developed by its decision. This improves the explainability of
  - detection of mental health conditions, we use a of LLMs, ChatGPT significantly outperforms
  - Text (Joulin et al., 2017), BERT/RoBERTa (De- still does not improve performance, possibly

## 6. Conclusion、局限与可复现性

- **结论段落线索**：few-shot examples in Sec. 2.2 are included in the In this work, we comprehensively studied LLMs zero-shot prompts. As the results in Table 4 show, on zero-shot/few-shot mental health analysis and with few-shot prompts, ChatGPT achieves a vari- the impact of different emotion-enhanced prompts. ance of 1.34 on DR, 7.21 on CLPsych15, and 31.93 We explored the potential of LLMs in explainable on Dreaddit, which are all significantly lower than mental health analysis, by explaining their predic- those of zero-shot prompts. These results prove tions via CoT prompting. We developed a reli- that expert-written examples can stabilize Chat- able annotation protocol for human evaluations of GPT鈥檚 predictions, because they can provide ac- LLM-generated explanations and benchmarked ex- curate references for the subjective mental health isting automatic evaluation metrics on the human detection and cause detection tasks. The few-shot annotations. Experiments demonstrated that men- solution is also e
- **局限/未来工作线索**：4) Limitations. Although its great potential, billion parameters) in our experiments.；ChatGPT bears limitations on inaccurate reasoning 3) ChatGPT. ChatGPT (gpt-3.5-turbo) is trained；analyze the potential and limitations of LLMs and of-Thought (CoT) (Wei et al., 2022), and distantly；teria of predictions hard to learn for ChatGPT in a We leave LLM-finetuning as future work. More
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Towards Interpretable Mental Health Analysis with Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
