# 132. Automatic Evaluation of Attribution by Large Language Models

> 逐篇阅读记录：第 132 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xiang Yue, Boshi Wang, Ziru Chen, Kai Zhang, Yu Su, Huan Sun
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-emnlp.307)
- **领域标签**：Attribution, Evaluation, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 12,878 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：A recent focus of large language model (LLM) development, as exemplified by generative search engines, is to incorporate external references to generate and support its claims.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction ods to automatically evaluate attribution and iden-；6 Related Work manner: 1) we explore how helpful other relevant；8 Limitations Acknowledgements
- **引言关键线索**：Generative large language models (LLMs) (Brown tify potential attribution errors are highly desired. et al., 2020; Ouyang et al., 2022; Chowdhery et al., Towards this goal, we take the first step by intro- 2022; OpenAI, 2023a,b, inter alia) often struggle ducing AttrScore (Figure 1), a framework designed with producing factually accurate statements, re- for automatic evaluation of attribution and identi- sulting in hallucinations (Ji et al., 2023). Recent fication of specific types of attribution errors. We efforts aim to alleviate this issue by augmenting propose a new problem formulation that catego- LLMs with external tools (Schick et al., 2023) such rizes attribution into three types: 1) attributable: as retrievers (Shuster et al., 2021; Borgeaud et al., the reference fully supports the generated state- 2022) and search engines (Nakano et al., 2021; ment; 2) extrapolatory: the refere
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：automatic evaluation: prompting LLMs and to make harmful health decisions. Similarly, in fine-tuning smaller LMs. The fine-tuning data finance, faulty investment advice attributed to a re- is repurposed from related tasks such as ques- liable source may cause substantial financial losses. tion answering, fact-checking, natural language inference, and summarization. We manually cu- To identify attribution errors, existing attributed rate a set of test examples covering 12 domains LLMs (Nakano et al., 2021; Thoppilan et al., 2022) from a generative search engine, New Bing. rely heavily on human evaluation, which is both Our results on this curated test set and simu- expensive and time-consuming. For instance, the lated examples from existing benchmarks high- average cost of annotating a single (query, answer, light both promising signals and challenges. reference) example is about $1 in Liu et al. (2023). We hope our problem formulation, 
- **方法与解释性关系**：该论文主要围绕 `Attribution, Evaluation, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - 鈥減artial鈥, or 鈥渘o support鈥, our fine-grained error mation comparisons, such as overlooking contex-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：31%, 81%, 50
 Percentage
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - In this paper, we investigate automatic evalu- thiness of LLMs, introducing significant safety
  - Our results on this curated test set and simu- expensive and time-consuming. For instance, the
  - ment directly contradicts the cited reference. Un- Our results indicate that both approaches show
  - Liu et al. (2023) that defines the degree of refer- further improvement. Major sources of evaluation
  - to a user鈥檚 query. Our task setting prioritizes one and system improvement.
  - GenSearch sets. The best-performing result in each setting is in bold. The results show both promising signals and
  - four types of LMs of various scales: Roberta and significantly outperforming other models. This

## 6. Conclusion、局限与可复现性

- **结论段落线索**：on researchers, e.g., 鈥淚s XX a co-author of the paper XX?鈥 https://github.com/lm-sys/FastChat 4619 AttrEval-Simulation AttrEval-GenSearch Setting Model (Size) Attri. Contra. Extra. Overall Attr. Contra. Extra. Overall Alpaca (7B) 50.0 4.0 1.4 33.6 50.7 8.6 3.6 34.3 Alpaca (13B) 48.3 5.6 2.2 33.5 50.6 6.1 19.3 34.7 Zero-shot Vicuna (13B) 46.3 8.3 21.6 34.6 54.4 13.3 26.1 41.4 ChatGPT 45.7 17.9 52.7 43.2 61.2 20.6 53.3 55.0 GPT-4 58.7 23.2 61.5 55.6 87.3 45.0 89.6 85.1 Alpaca (7B) 45.4 8.2 9.6 31.9 49.6 5.2 13.5 37.2 Alpaca (13B) 38.9 20.1 2.2 33.1 50.5 10.3 5.6 34.8 Few-shot Vicuna (13B) 35.4 37.2 0.3 32.6 50.6 9.1 8.4 34.1 ChatGPT 46.6 27.6 35.8 39.2 62.6 26.8 49.5 53.3 GPT-4 61.1 31.3 68.8 60.0 85.2 53.3 88.9 84.3 Roberta (330M) 62.5 54.6 74.7 65.0 47.2 25.2 62.3 49.8 GPT2 (1.5B) 63.6 54.6 71.9 63.5 51.1 18.6 60.7 47.4 T5 (770M) 45.9 57.1 71.6 59.1 58.5 24.3 72.5 61.6 Flan-T5 (770M) 57.3 50.1 70.5 59.3 64.3 27.6 72.9 64.5 Flan-T5 (3B) 48.1 48.7 67.1 55.7 77.7 44.4 80.0 75.2 Fine-tuned
- **局限/未来工作线索**：language inference (NLI), and summarization. For potential directions for future work, we hope our；A100 80GB GPUs with a maximum of 512 tokens. of bias (see Limitations Section 8).；8 Limitations Acknowledgements；While acknowledging current limitations, sev- ral Language Processing and the 9th International
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Automatic Evaluation of Attribution by Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
