# 025. Mitigating Privacy Seesaw in Large Language Models: Augmented Privacy Neuron Editing via Activation Patching

> 逐篇阅读记录：第 25 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Xinwei Wu, Weilong Dong, Shaoyang Xu, Deyi Xiong
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.findings-acl.315)
- **领域标签**：Mechanistic, Hidden, LLM, Activation, Control
- **本地 PDF 文本规模**：约 8,057 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Mechanistic为主线，结合论文摘要中的核心设定：Protecting privacy leakage in large language models remains a paramount challenge.In this paper, we reveal Privacy Seesaw in LLM privacy protection via neuron editing, a phenomenon where measures to secure specific priva
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction as ChatGPT with meticulously crafted prompts, un-；3 Preliminary；6 Future Work the computational efficiency of neuron localization；2022. Gpt-neox-20b: An open-source autoregressive Mor Geva, Roei Schuster, Jonathan Berant, and Omer；2022. What does it mean for a language model；2022. Training language models to follow instruc- Conference on Empirical Methods in Natural Lan-
- **引言关键线索**：derscoring the urgency of privacy protection for Large language models have demonstrated out- LLMs (Li et al., 2023a). standing capabilities in natural language under- In order to protect privacy of LLMs, machine standing and generation, significantly advancing unlearning and neuron-based methods have been downstream natural language processing (NLP) proposed. The former aims to make LLMs for- tasks (Brown et al., 2020; Chung et al., 2022; get targeted datasets through fine-tuning on small Ouyang et al., 2022; Achiam et al., 2023). How- batches of data. Ishibashi and Shimodaira (2023) ever, LLMs trained on vast amounts of Internet render private information harmless through Sani- data encounter critical security and privacy chal- tization Tuning. Jang et al. (2022) reduce privacy lenges in real-life application scenarios (Shen et al., leakage risks by reversely learning the gradient of 2
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：work designed to well balance model perfor- privacy leakage risk of the targeted private data (texts mance with privacy protection. The proposed 1-5), it paradoxically increases the risk for certain non- APNEAP augments collected private data by targeted private data (text 7). automatically synthesizing new private data, which deactivates the first trigger to the privacy seesaw issue. Additionally, it adapts activation data for LLMs often contain sensitive or unautho- patching to privacy neuron editing for switch- rized information, which is subjected to limited ing off the second trigger to the privacy seesaw scrutiny because of its massiveness and confiden- problem. Experimental results show that the proposed APNEAP is capable of alleviating tiality (Piktus et al., 2023; Li et al., 2023a). Second, the privacy seesaw phenomenon and offers a LLMs tend to memorize training data, including more stable and reliable approach to privacy uniq
- **方法与解释性关系**：该论文主要围绕 `Mechanistic, Hidden, LLM, Activation, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Wu et al, Baselines To evaluate the, Differential Privacy What Causes, Privacy Seesaw, Our investi-, DEPN, In comparison with Table, APNEAP and baselines. The, APNEAP compared to baselines, Due, Table 4, Comparison of performance metrics
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - formance than strong baselines.
  - tion over strong baselines, while maintaining high alignment of knowledge neurons. Wu et al. (2023)
  - E. Baselines To evaluate the performance of the of privacy seesaw.
  - pare with three baselines. Differential Privacy What Causes the Privacy Seesaw? Our investi-
  - sanitized dataset. DEPN: the baseline of privacy no instances of increased privacy exposure risk
  - In comparison with Table 2a, these results suggest

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - problem. Experimental results show that the
  - standing and generation, significantly advancing unlearning and neuron-based methods have been
  - avoid significant impact on the model performance, neuron editing (Wu et al., 2023), fine-tuning on tar-
  - sents a significant advancement in model editing,
  - large. In our experiments, we find that an insuffi-
  - and 90,316 email instances in the Enron dataset, dataset, we find 977 instances with increased leak-
  - lected 5% the data of Enron and MIMIC as the datasets and significantly protect the targeted sub-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：lighted the importance of balancing model perfor- In this paper, we have identified the privacy seesaw mance with protection strength (Abadi et al., 2016; phenomenon as a previously underexplored prob- Habernal, 2021). This balance is particularly chal- lem in LLM privacy protection, where efforts to lenging, as demonstrated in works on differential protect certain private data instances inadvertently privacy (Shi et al., 2021; Wu et al., 2022). In post- increase exposure risks for others. We pinpoint the processing privacy protection for large language amount of targeted privacy data and the number models, it鈥檚 impractical to have a complete dataset of privacy neurons being edited as key triggers of of private information. Protecting only a subset of this phenomenon. To tackle this, we proposed AP- private data fails to cover unknown private data, NEAP, effectively balancing model performance leading to a privacy seesaw effect. Future research with privacy protection and significantly
- **局限/未来工作线索**：data (Text 7). This finding highlights the limitations language models are categorized into three stages:；data, but also from the limitation of DEPN method, require the longest processing time, followed by；6 Future Work the computational efficiency of neuron localization；Limitations Nicholas Carlini, Daphne Ippolito, Matthew Jagielski,
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Mitigating Privacy Seesaw in Large Language Models: Augmented Privacy Neuron Editing via Activation Patching”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
