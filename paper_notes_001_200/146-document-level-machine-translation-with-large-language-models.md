# 146. Document-Level Machine Translation with Large Language Models

> 逐篇阅读记录：第 146 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Longyue Wang, Chenyang Lyu, Tianbo Ji, Zhirui Zhang, Dian Yu, Shuming Shi, Zhaopeng Tu
- **发表 venue / date**：EMNLP / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.emnlp-main.1036)
- **领域标签**：Probing, Evaluation, LLM, Behavior, Evaluate
- **本地 PDF 文本规模**：约 9,622 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Large language models (LLMs) such as ChatGPT can produce coherent, cohesive, relevant, and fluent answers for various natural language processing (NLP) tasks.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction modelling specific discourse phenomena (Wang；5 Analysis of Large Language Models；6 Conclusion and Future Work We list the main limitations of this work as follows:；2020. Statistical power and translationese in ma-；2017 Conference on Empirical Methods in Natural；2011. Document-level consistency verification in ma-；8. We find that the corresponding two-tailed p-；0 Translation output is unrelated to the source Output is unrelated to previous or following
- **引言关键线索**：et al., 2016; Voita et al., 2018; Wang et al., 2018a,b, In the past several years, machine translation (MT) 2019; Voita et al., 2019b; Wang et al., 2023b). The has seen significant advancements with the intro- most popular large language model (LLM) 鈥 Chat- duction of pre-trained models such as BERT (De- GPT3 shows the ability of maintaining long-term vlin et al., 2018), GPT-2 (Radford et al., 2019), and coherence and consistency in a conversation by 鈭 Equal contribution. conditioning on previous conversational turns. Ad- 1 The protocol employed in this work was approved by the Tencent Institutional Review Board (IRB). 3 https://chat.openai.com. All corresponding results 2 We release our data and annotations at https://github. were obtained from GPT-3.5 and GPT-4 in March 2023. The com/longyuewangdcu/Document-MT-LLM. reproducibility is discussed in Section Limitation. 16646 Proceedings o
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：ysis of Discourse Modelling Abilities, where we further probe discourse knowledge encoded Figure 1: An example of translating a document- in LLMs and shed light on impacts of training level text from English to Chinese using GPT-4 (Date: techniques on discourse modeling. By evalu- 2023.03.17). We highlight the discourse phenomena ating on a number of benchmarks, we surpris- using figures and lines, which are invisible to GPT-4. ingly find that LLMs have demonstrated supe- rior performance and show potential to become a new paradigm for document-level translation: T5 (Raffel et al., 2020). These models have demon- 1) leveraging their powerful long-text modeling strated impressive performance on MT (Zhu et al., capabilities, GPT-3.5 and GPT-4 outperform 2020; Guo et al., 2020; Xue et al., 2021). However, commercial MT systems in terms of human most of the existing work has focused on sentence- evaluation;1 2) GPT-4 demonstrates a stronger
- **方法与解释性关系**：该论文主要围绕 `Probing, Evaluation, LLM, Behavior, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Comparison of Translation Models, Comparison of Advanced Translation, Models, Dataset, MT prod-, We compare variant models, ChatGPT et al, However, As seen, ZPT, Base is a sentence-level, Table 4, Comparison between commercial MT, LLM applications on Chinese, Comparison of Different Prompts, ChatGPT, In this section
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Comparison of Translation Models, where we
  - 鈥 Comparison of Advanced Translation Models: 2.1 Dataset
  - systematic comparison of commercial MT prod-
  - gated. We compare variant models of ChatGPT et al., 2019b) that specifically targets discourse phe-
  - However, this is not a strict comparison because As seen, we also report the average length of
  - translation (A ZPT) and consistency of terminology translation (C TT). Base is a sentence-level baseline system

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：0.6 points, 1.9 points
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - capabilities, GPT-3.5 and GPT-4 outperform 2020; Guo et al., 2020; Xue et al., 2021). However,
  - has seen significant advancements with the intro- most popular large language model (LLM) 鈥 Chat-
  - conclusions, we also conduct a human evaluation prompts to trigger the document-level translation
  - two dimensions: general quality and discourse awareness (detailed in Table 12). The significant test is detailed in
  - phenomena, P3 outperforms other candidates with Table 4 shows the results. When evaluated using
  - significantly better than MT systems in terms of can effectively leverage document-level context for
  - indicates a significant difference (p < 0.001) between

## 6. Conclusion、局限与可复现性

- **结论段落线索**：ducted ChatGPT in March 28鈭31 2023 with the official notice 鈥渢he training data is up to Septem- ber 2021鈥.4 Different from previous work that leaned on dated or single-type datasets to assess ChatGPT鈥檚 capabilities (Jiao et al., 2023; Lu et al., 2023), we carefully chosen both lastst, public and diverse testsets to mitigate the risks associated with data contamination. Taking Probing Discourse Knowledge (Section 5.1) for example, while the contrastive testset used for the prediction task orig- inated in 2019, the evaluation on the explanation task remains devoid of public references. Our con- clusions are comprehensively made by considering both prediction and explanation results, balancing out any potential data contamination concerns. De- spite precautions, there remains a risk of data con- tamination, given that publicly available datasets are easily incorporated into LLM training (e.g. pre- training, SFT, or RLHF). A better way is to con- sistently integrate and employ the latest d
- **局限/未来工作线索**：com/longyuewangdcu/Document-MT-LLM. reproducibility is discussed in Section Limitation.；(see Section Limitation.). We establish two sets ability of ChatGPT.；translations. However, human takes into account leave this fine-grained comparison for future work.；+FeedME-1). When further adding PPO method, In our future work, we plan to explore more
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Document-Level Machine Translation with Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
