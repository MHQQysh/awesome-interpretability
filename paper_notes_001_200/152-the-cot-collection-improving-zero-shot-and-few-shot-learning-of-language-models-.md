# 152. The CoT Collection: Improving Zero-shot and Few-shot Learning of Language Models via Chain-of-Thought Fine-Tuning

> 逐篇阅读记录：第 152 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Seungone Kim, Segyeong Joo, Doyoung Kim, Joel Jang, Seonghyeon Ye, Jamin Shin, Minjoon Seo
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.782)
- **领域标签**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 17,627 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Language models (LMs) with less than 100B parameters are known to perform poorly on chain-of-thought (CoT) reasoning in contrast to large LMs when solving unseen tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction tuning results in poor CoT results across smaller；4 Experiments；5 Analysis of of CoT Fine-tuning；9 CoT Data；2022 Conference of the North American Chapter of；1241. Thomas L Griffiths, Yuan Cao, and Karthik
- **引言关键线索**：LMs (Longpre et al., 2023). In general, the com- Language models (LMs) pre-trained on massive munity still lacks a comprehensive strategy to fully text corpora can adapt to downstream tasks in both leverage CoT prompting to solve multiple unseen zero-shot and few-shot learning settings by incorpo- novel tasks in the context of smaller LMs. rating task instructions and demonstrations (Brown To bridge this gap, we present the C OT C OL - et al., 2020; Wei et al., 2021; Sanh et al., 2021; LECTION , an instruction tuning dataset that aug- Mishra et al., 2022; Wang et al., 2022b; Iyer et al., ments 1.84 million rationales from the FLAN Col- 2022; Liu et al., 2022b; Chung et al., 2022; Long- lection (Longpre et al., 2023) across 1,060 tasks. pre et al., 2023; Ye et al., 2023). One approach that We fine-tune Flan-T5 (3B & 11B) using C OT C OL - LECTION and denote the resulting model as CoT- 鈭 d
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：We fine-tune Flan-T5 (3B & 11B) using C OT C OL - LECTION and denote the resulting model as CoT- 鈭 denotes equal contribution. Work was done while Seun- gone was interning at NAVER AI Lab. 鈥 T5. We perform extensive comparisons of CoT-T5 Corresponding authors 1 https://github.com/kaistAI/ and Flan-T5 under two main scenarios: (1) zero- CoT-Collection shot learning and (2) few-shot learning. 12685 Proceedings of the 2023 Conference on Empirical Methods in Natural Language Processing, pages 12685鈥12708 December 6-10, 2023 漏2023 Association for Computational Linguistics In the zero-shot learning setting, CoT-T5 (3B & 2 Related Works 11B) outperforms Flan-T5 (3B & 11B) by +4.34% 2.1 Chain-of-Thought (CoT) Prompting and +2.60% on average accuracy across 27 datasets from the Big Bench Hard (BBH) benchmark (Suz- Wei et al. (2022b) propose Chain of Thought (CoT) gun et al., 2022) when evaluated with CoT prompt- Prompting, a technique that trigg
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：We perform extensive comparisons, CoT-T5, Flan-T5, LMs to outperform larger, T5-LM, Raffel LMs and, FLAN Collection and, CoT fine-tuned on instruction, The best comparable, Flan Collection is more, LMs addition to the, GPT-3, When compared with C, OT FT, T0-3, Training Setup We compare, This results in CoT-T5, CoT
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - 鈥 T5. We perform extensive comparisons of CoT-T5
  - uating Flan-T5, we compare the performances of tasks enables smaller LMs to outperform larger
  - of different baselines such as (1) T5-LM (Raffel LMs and (2) training with FLAN Collection and
  - report the direct performance of the baselines since they were not CoT fine-tuned on instruction data. The best comparable
  - Flan Collection is more effective in enabling LMs addition to the baselines from the previous section
  - fitting to the output label as baseline models. GPT-3 (175B) 0.0 4.4 10.8 6.8 0.8

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：2x, 10x, 29 times, 78 times, 63 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - show that CoT fine-tuning Flan-T5 (3B & 11B) results in significant computational cost and acces-
  - we report an average improvement of +4.34%
  - we show that instruction tuning with C OT C OL - nales (denoted as CoT fine-tuning) and applying
  - tasks, resulting in an improvement of +2.24% ever, solving a single task does not adequately ad-
  - outperforming ChatGPT utilizing demonstra-
  - 11B) outperforms Flan-T5 (3B & 11B) by +4.34%
  - ing. During ablation experiments, we show that generate a rationale before the answer. By gener-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：The results are shown in Figure 3, where sur- In this work, we show that augmenting rationales prisingly, only using 10K instances across 1,060 from an instruction tuning data using LLMs (Open tasks obtains better performance compared to us- AI Codex), and CoT fine-tuning could improve ing 180K instances across 9 tasks. This shows that the reasoning capabilities of smaller LMs. Specif- maintaining a wide range of tasks is more crucial ically, we construct C OT C OLLECTION, a large- compared to increasing the number of instances. scale instruction-tuning dataset with 1.84M CoT rationales extracted across 1,060 NLP tasks. With 5.2 In-domain Task Accuracy of CoT-T5 our dataset, we CoT fine-tune Flan-T5 and obtain It is well known that LMs that are fine-tuned on a CoT-T5, which shows better zero-shot generaliza- wide range of tasks suffer from catastrophic forget- tion performance and serves as a better base model ting (Chen et al., 2020; Jang et al., 2021, 2023), when training with few nu
- **局限/未来工作线索**：C OT-T5, we use Flan-T5 as our base model and ting during CoT fine-tuning to future work.；et al., 2023) could also be explored in future work.；future work (Wang et al., 2023). Moreover, vari- nologies, Volume 1 (Long and Short Papers), pages；important line of future work. While Shi et al. arXiv:2111.10952.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“The CoT Collection: Improving Zero-shot and Few-shot Learning of Language Models via Chain-of-Thought Fine-Tuning”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
