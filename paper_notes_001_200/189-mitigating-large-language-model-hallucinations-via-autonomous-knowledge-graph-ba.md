# 189. Mitigating Large Language Model Hallucinations via Autonomous Knowledge Graph-Based Retrofitting

> 逐篇阅读记录：第 189 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xinyan Guan, Yanjiang Liu, Hongyu Lin, Yaojie Lu, Ben He, Xianpei Han, Le Sun
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/29770/31326)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 6,395 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：Incorporating factual knowledge in knowledge graph is regarded as a promising approach for mitigating the hallucination of large language models (LLMs).
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2) By verifying the facts used during reasoning via chain-；4) Our framework can work well on compact size LMs, Error Analysis
- **引言关键线索**：requires an intermediate knowledge about 鈥淣icolas Chopin Large Language Models (LLMs) have gained increasing was born on April 15, 1771鈥. However, such information prominence in artificial intelligence. The emergence of potent does not refer to entities appearing in the query. As a result, models such as ChatGPT (OpenAI 2022) and LLaMA (Tou- previous approaches are inadequate in addressing the factual vron et al. 2023) has led to substantial influences on many hallucination appearing in the reasoning processes of LLMs. areas like society, commerce, and research. However, LLMs In this paper, we propose Knowledge Graph-based still suffer from severe factual hallucination problems, i.e., Retrofitting (KGR), a new framework that incorporates LLMs LLMs can frequently generate unsupported false statements with KGs to mitigate factual hallucination during the entire regarding factual informatio
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：nation of large language models (LLMs). Existing methods stantial amount of high-quality factual information, which usually only use the user鈥檚 input to query the knowledge graph, can significantly alleviate factual hallucination if incorpo- thus failing to address the factual hallucination generated rated with LLMs. For example, in Figure 1, we can retrofit by LLMs during its reasoning process. To address this prob- the erroneous statement 鈥淣icolas Chopin was born on June lem, this paper proposes Knowledge Graph-based Retrofitting 17, 1771鈥 by referring to the provided factual knowledge (KGR), a new framework that incorporates LLMs with KGs 鈥(Nicolas Chopin, date of birth, 1771-04-15T00:00:0)鈥 in to mitigate factual hallucination during the reasoning process Wikidata. Recent work has focused on integrating LLMs by retrofitting the initial draft responses of LLMs based on the with KGs by retrieving the entities in the query within knowl
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Simple Question Mintaka HotpotQA, Baseline, CoT 14.0, We compared our KGR, Compared with the CoT, CRITIC, KGR frame-, KGR, We compare KGR with, CoT and QKR on, Simple Question, Mintaka
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Simple Question Mintaka HotpotQA Baseline
  - CoT 14.0/21.9 26.0/28.3 12.0/19.6 We compared our KGR framework with the following meth-
  - which encompasses structured data from various sources such Compared with the CoT and CRITIC, our KGR frame-
  - eralizability of KGR. We compare KGR with the strong
  - baselines CoT and QKR on Simple Question, Mintaka,

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - usually only use the user鈥檚 input to query the knowledge graph, can significantly alleviate factual hallucination if incorpo-
  - KGR can significantly improve the performance of LLMs on knowledge relevant to entities explicitly mentioned within
  - verifying by KGs. As shown in Figure 1, given the draft that KGR can significantly mitigate the hallucination and
  - selector to identify fact triples relevant to the draft response. which can significantly impact the quality of the generated
  - nificantly improve the performance of LLMs on factual QA 2023b) introduces an adaptive method for determining the
  - text-davinci-003 and ChatGPT (gpt-3.5-turbo-0301) to see As shown in Table 1, our method demonstrates significant
  - data2 (Vrandec虒ic虂 and Kro虉tzsch 2014) as our knowledge base, and achieve significant improvements on 3 datasets.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：selection were conducted on a sample of 50 instances from In this paper, we propose a knowledge graph-based Simple Question with different chunk sizes. b). Results for retrofitting framework that effectively mitigates factual hallu- fact selection were conducted on a sample of 50 instances cination during the reasoning process of LLMs based on the from Simple Question with different retrieved triple numbers. factual knowledge stored in KGs. Experiment results show that KGR can significantly im- prove the performance of LLMs on factual QA benchmarks Impact of Chunk Size&Numbers of Retrieved especially when involving complex reasoning, which demon- Triples strates the necessity and effectiveness of KGR in mitigating hallucination and enhancing the reliability of LLMs. As for As discussed above, considering the limitation of maximum future work, we plan to improve the effectiveness in each input length for LLMs, we partition the retrieved triples into step of our KGR framework. chunks for
- **局限/未来工作线索**：and retrieval capabilities. One major limitation of these ap-；As discussed above, considering the limitation of maximum；future work, we plan to improve the effectiveness in each
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Mitigating Large Language Model Hallucinations via Autonomous Knowledge Graph-Based Retrofitting”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
