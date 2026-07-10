# 110. Null-Shot Prompting: Rethinking Prompting Large Language Models With Hallucination

> 逐篇阅读记录：第 110 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Pittawat Taveekitworachai, Febri Abdullah, Ruck Thawonmas
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.740)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 24,334 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：This paper investigates an interesting phenomenon where we observe performance increases in large language models (LLMs) when providing a prompt that causes and exploits hallucination.We propose null-shot prompting, a co
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Evaluation of Null-Shot Prompting；5 Hallucination Detection tion improves the hallucination detection abilities；361. Proceedings of the 2017 Conference on Empirical；2023. Jailbroken: How Does LLM Safety Train- Yulong Chen, Longyue Wang, Anh Tuan Luu, Wei；3 Sonnet, or Claude 3 Opus from our main experi-；3 Models
- **引言关键线索**：research groups have proposed that hallucination Hallucination of generative models, in a broad may be regarded as a way for LLMs to be creative sense, is defined as a situation where there is con- (Huang et al., 2023; Rawte et al., 2023b; Jiang et al., flicting information, either with facts, established 2024). Given that hallucination may be inevitable knowledge, intents, or previously generated or pro- and is an innate property of LL Ms, instead of fo- vided content, within their context window (Ji et al., cusing solely on mitigating hallucination, which 2023; Zhang et al., 2023; Huang et al., 2023; Rawte is still crucial, an alternative approach is to take et al., 2023b). Hallucination existed even before the advantage of this property instead. recent widespread usage of large language models This perspective must hold some value, as in a (LLMs) (Ji et al., 2023). However, it has bec
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：independent clauses are joined by the conjunction "but." The sentence "All the clutter in the house excited Leslie but not Derrick because cleaning energized Leslie very much" is an example of a complex sentence with an independent ately instruct LLMs to reference a null, non- The first independent clause tells us that Leslie was excited by the clutter in the house. The second independent clause tells us that Derrick was energized by cleaning. The two clause and a dependent clause. The independent clause is "All the clutter in the house excited Leslie." The dependent clause is "because cleaning energized Leslie very much." existent, section. We evaluate null-shot prompt- clauses are related because they both describe how the characters in the story feel about clutter. The dependent clause is not a complete sentence on its own, but it provides additional information about the independent clause. In this case, the dependent clause ing acr
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 1. We note, For cases where null-shot, Henceforth, MATH dataset, Num, On the other hand, We compare placing the, For the, Turbo model. We compare
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - zero-shot prompting baseline in Table 1. We note
  - changes. For cases where null-shot prompting or its reasoning variant show improvement over the baseline, 鈭 , 鈭椻垪 ,
  - to the zero-shot prompting baseline. Henceforth, for all the tables presenting results from the MATH dataset, Num.
  - formance baseline compared to the previous sec- the original prompts from the evaluation set to suit
  - null-shot prompting is effective with chat-tuned compared to a strong 0CoT prompting baseline.
  - ever, we change the baseline to zero-shot chain-of- effect sizes. On the other hand, there are 40 im-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：11.28%, 10.95%, 10.1%, 1.85%, 3.64%, 7.01%, 6.97%, 38.46%, 28.97%, 1.93%, 2.13%, 44.62%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and find several cases of performance improve-
  - of 34 combinations that show improvements from
  - We propose a null-shot phrase for null-shot prompt- null-shot prompting, 20 are statistically significant,
  - placing it at the end of the prompt, as demonstrated improve the performance of PaLM 2, both text and
  - in Section 7.1. The original optimized prompt for chat generation. We observe great improvement in
  - improvement in reading comprehension. However, On the other hand, we observe that null-shot
  - chances of hallucination through improved recall previously discussed.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：hallucination in prompts may not only affect Recent research has suggested that hallucination is LLMs but, in certain cases, enhance their per- instead a feature of LLMs (Bai et al., 2024) and is formance. to be expected from calibrated LLMs (Kalai and
- **局限/未来工作线索**：acknowledge the limitation of our current approach & probability, which shows performance increase；coupled with the model. Therefore, future work necessary. Additionally, we observe moderate per-；could not do due to limitations of our computa- where we observe the improvement more from in-；Limitations for LLMs is an active area of research and there
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Null-Shot Prompting: Rethinking Prompting Large Language Models With Hallucination”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
