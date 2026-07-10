# 057. A Multitask, Multilingual, Multimodal Evaluation of ChatGPT on Reasoning, Hallucination, and Interactivity

> 逐篇阅读记录：第 57 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Yejin Bang, Samuel Cahyawijaya, Nayeon Lee, Wenliang Dai, Dan Su, Bryan Wilie, Holy Lovenia, Ziwei Ji, et al.
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.ijcnlp-main.45)
- **领域标签**：Evaluation, MLLM, Reasoning, Behavior, Detect
- **本地 PDF 文本规模**：约 26,907 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Yejin Bang, Samuel Cahyawijaya, Nayeon Lee, Wenliang Dai, Dan Su, Bryan Wilie, Holy Lovenia, Ziwei Ji, Tiezheng Yu, Willy Chung, Quyet V.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction swering, reasoning, summarization, machine trans-；4 Factuality and Hallucination；6 Conclusion and Discussion；2023. Chatgpt vs satya nadella over biryani: The chat- Christian Wibisono, Ade Romadhony, Karissa Vin-；2022. Qa dataset explosion: A taxonomy of nlp Commonsense reasoning for natural language under-；3. If the generated image contains errors, we；2. Query for the post-editing using the following；2. To refine the summary, we simply input an-
- **引言关键线索**：ChatGPT is a successor of the large language lation, sentiment analysis, language identification, model (LLM) InstructGPT (Ouyang et al., 2022) task-oriented dialogue, and misinformation detec- with a dialog interface that is fine-tuned using the tion. We evaluate its multilingual performance as Reinforcement Learning with Human Feedback well as vision-language multimodal abilities. With (RLHF) (Christiano et al., 2017) approach. Chat- additional experiments, we also quantitatively eval- GPT has gathered 100 million monthly active users uated its primary limitations in reasoning and hal- in such a short period of time (Hu, 2023) and is lucination. In addition, we conducted experiments being used by businesses and consumers alike for to test its multi-turn interactivity as a means for a myriad of mostly textual tasks. One reason for its better prompt engineering. We aimed to provide unpre
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：abilities. Another reason is that its dialog interface tatively evaluating interactive LLMs such as ChatGPT using publicly available data sets, us- allows users to interact with the underlying LLM ing 23 data sets covering 8 different common more effectively and efficiently via interactive chats NLP application tasks. We extensively evaluate that are akin to multi-turn prompting. the multitask, multilingual, and multi-modal as- However, despite its powerful abilities, anec- pects of ChatGPT based on these data sets and dotal reports on ChatGPT consistently showed a newly designed multimodal dataset. We find that ChatGPT outperforms LLMs with zero- remaining challenges - for example, it fails in shot learning on most tasks and even outper- some elementary mathematical (Gilson et al., 2022; forms fine-tuned models on some tasks. We Goldberg, 2023; Frieder et al., 2023; Choi et al., find that it is better at understanding non-Latin 2023; D
- **方法与解释性关系**：该论文主要围绕 `Evaluation, MLLM, Reasoning, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：GPT2, Categories Testset Result tively, In comparison to its, Interactivity Compared with the, LLMs, Gozalo-Brizuela ducted for comparison, Moreover, OpenAI, F1 score as the, We compare the, Multitask Evaluation of ChatGPT, For a fairer comparison, Table 14, StepGame, Basic, ChatGPT, We make the
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - ability as shown from the results in QA, and 3) are relatively low compared with fine-tuned GPT2.
  - Categories Testset Result tively. In comparison to its previously reported
  - Interactivity Compared with the previous LLMs, common issues with these models, such as fairness,
  - comparison corpus, evaluation, and detection. arXiv
  - been overviews of existing LLMs (Gozalo-Brizuela ducted for comparison. Moreover, OpenAI鈥檚 text-
  - F1 score as the evaluation metric. We compare the

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：0.000        X, 34%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - a newly designed multimodal dataset. We find
  - that ChatGPT outperforms LLMs with zero-
  - ation step. Moreover, we find that ChatGPT
  - with the underlying LLM to improve its perfor-
  - https://github.com/HLTCHKUST/chatgpt-evaluat can improve outcomes with interactivity. To the
  - clear. Thus, any benchmarking exercise cannot session. There is also significant performance im-
  - For 9/13 NLP datasets, ChatGPT outperforms pre-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：of the time, respectively. For instance, ChatGPT judgment based on given evidence or past experi- cannot generate the exact shape of the maple leaf in ence and observations (Rogers et al., 2022; Wason the Canadian flag while it gets the layout and color and Johnson-Laird, 1972; Huang and Chang, 2022). correctly (Figure 3). This is a natural defect of We first investigate basic reasoning skills with bAbI text-only language models as they never see actual tasks (Weston et al., 2016b), 30 examples each visual data and textual data is usually conceptual. from task 15 (inductive) and task 16 (deductive). One major investigation is that ChatGPT is a lazy
- **局限/未来工作线索**：GPT has gathered 100 million monthly active users uated its primary limitations in reasoning and hal-；鈭 tioned above and limitations, as well as how they；observe several limitations of ChatGPT: 1) limited of high quality with fluent response generation and；structured database. We observed the limitations GPT performance with the language resource cate-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“A Multitask, Multilingual, Multimodal Evaluation of ChatGPT on Reasoning, Hallucination, and Interactivity”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
