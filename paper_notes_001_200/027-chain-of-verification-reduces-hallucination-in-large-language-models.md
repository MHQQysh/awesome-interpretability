# 027. Chain-of-Verification Reduces Hallucination in Large Language Models

> 逐篇阅读记录：第 27 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Shehzaad Dhuliawala, Mojtaba Komeili, Jing Xu, Roberta Răileanu, Xian Li, Aslı Çelikyılmaz, Jason Weston
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.212)
- **领域标签**：Analysis, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 8,113 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Generation of plausible yet incorrect factual information, termed hallucination, is an unsolved issue in large language models.We study the ability of language models to deliberate on the responses they give in order to
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction facts than those in the original longform answer,；1. Generate Baseline Response: Given a query,；4. Generate Final Verified Response: Given the；1. Baseline Response；1. Hillary Clinton - former secretary of state and former Democratic presidential nominee；2. Donald Trump - former president of the United States；3. Michael Bloomberg - former Mayor of New York City and former Democratic presidential candidate；4. Final Verified Response
- **引言关键线索**：Large Language Models (LLMs) are trained on and hence improve the correctness of the overall huge corpora of text documents with billions of response. We study variations on this recipe across tokens of text. It has been shown that as the num- a range of tasks: from list-based questions, closed ber of model parameters is increased, performance booked QA and longform text generation. We first at tasks such as closed book QA improve in accu- propose a joint approach for generating the entire racy, and larger models can generate more correct verification chain left-to-right, which improves per- factual statements (Radford et al., 2019; Petroni formance and decreases hallucinations compared et al., 2019). However, even the largest models to the baseline language model. However, models can still fail, particularly on lesser known torso and that attend to existing hallucinations in the con- ta
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：drafts an initial response; then (ii) plans veri- line of research to study how and when language- fication questions to fact-check its draft; (iii) model-based reasoning can be used to reduce hal- answers those questions independently so the lucinations. We develop an approach, called Chain- answers are not biased by other responses; and of-Verification (CoVe) which, given an initial draft (iv) generates its final verified response. In ex- periments, we show C OV E decreases hallucina- response, first plans verification questions to check tions across a variety of tasks, from list-based its work, and then systematically answers those questions from Wikidata, closed book Multi- questions in order to finally produce an improved SpanQA and longform text generation. revised response. We find that independent veri- fication questions tend to provide more accurate
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：However, Generate Baseline Response, Given a query, Figure 1, Chain-of-Verification, CoVe, Given a user query, We describe these steps, An tual claims in, For, Baseline Response and Mexico, Given such baseline generations, LLM
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - et al., 2019). However, even the largest models to the baseline language model. However, models
  - 1. Generate Baseline Response: Given a query,
  - Figure 1: Chain-of-Verification (CoVe) method. Given a user query, a large language model generates a baseline
  - We describe these steps in more detail below. An tual claims in the original baseline response. For
  - 3.1 Baseline Response and Mexico from 1846 to 1848鈥, then one possi-
  - as the baseline we wish to improve in our experi- templated and the language model is free to phrase

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Abstract on their ability to reason. Improved performance
  - periments, we show C OV E decreases hallucina- response, first plans verification questions to check
  - questions from Wikidata, closed book Multi- questions in order to finally produce an improved
  - SpanQA and longform text generation. revised response. We find that independent veri-
  - Large Language Models (LLMs) are trained on and hence improve the correctness of the overall
  - at tasks such as closed book QA improve in accu- propose a joint approach for generating the entire
  - racy, and larger models can generate more correct verification chain left-to-right, which improves per-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：hallucinations are wrong if queried specifically for that individual fact, independent of the rest of the We introduced Chain-of-Verification (CoVe), an ap- longform generation, see Figure 1, Figure 3, and proach to reduce hallucinations in a large language section 10. This can be seen quantitatively on the model by deliberating on its own responses and Wikidata task, where only 鈭17% of the Llama self-correcting them. In particular, we showed that few-shot baseline answer entities are correct in list- models are able to answer verification questions based questions. However, when querying each with higher accuracy than when answering the orig- individual entity via a verification question, we inal query by breaking down the verification into a find 鈭70% are correctly answered. set of simpler questions. Secondly, when answer- ing the set of verification questions, we showed Open LLM-based verification questions outper- that controlling the attention of the model so that form yes/no-base
- **局限/未来工作线索**：Limitations arXiv:2307.04507.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Chain-of-Verification Reduces Hallucination in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
