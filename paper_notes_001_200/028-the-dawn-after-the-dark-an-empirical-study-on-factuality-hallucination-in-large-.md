# 028. The Dawn After the Dark: An Empirical Study on Factuality Hallucination in Large Language Models

- **作者 / venue**：Junyi Li, Jie Chen, Ruiyang Ren, Xiaoxue Cheng, Xin Zhao, Jian-Yun Nie, Ji-Rong Wen；ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.586/)
- **任务**：系统研究 LLM hallucination 的检测、来源和缓解

## 1. Introduction

幻觉不是一个单一错误：模型可能生成训练知识之外的内容、与给定 context 矛盾的内容、或在开放域问题上编造事实。已有论文往往只研究一个任务或一个 mitigation method，因此难以回答 hallucination 是由数据、模型、训练、解码还是检索造成的。

这篇工作以系统实证为目标，围绕三条问题展开：如何检测 hallucination？它来自哪里？哪些训练、检索和 decoding 技术可以缓解？

## 2. Method / benchmark

- 扩展 HaluEval 思路，构建更全面的 hallucination benchmark。
- 用多领域、多任务和多模型测试输出中的 hallucinated statements。
- 研究训练阶段、模型规模、指令对齐、解码策略和 retrieval augmentation 对事实性的影响。
- 指标包括 hallucination rate、micro hallucination rate（MiHR）、事实性/有效回答比例以及模型之间的差异。

## 3. Baseline 与对比

- 不同 LLM（包括 ChatGPT、Claude 等）比较模型间事实性差异。
- 不同 decoding：temperature、sampling、beam/contrastive 等，用来研究 diversity-factuality trade-off。
- 纯生成 vs retrieval-augmented generation，检验外部知识是否减少幻觉。
- 检测方法与人工/自动判断对照，避免把 hedge/拒答误判为低 hallucination。

## 4. Findings

- 开放域 LLM 即使语言能力很强，仍会生成事实错误；模型的训练和对齐并未自动解决 hallucination。
- Claude 2 在部分设置中过度谨慎，会用 hedge/拒答降低 hallucination rate，同时也降低 valid response rate；因此低幻觉率不能单独等同于高质量。
- decoding 体现明显的 diversity-factuality trade-off：更追求多样性可能提高事实错误，过度保守又会损失有效回答。
- retrieval augmentation 在多个场景能减轻 hallucination，但检索质量和模型是否正确使用证据决定最终效果。
- 论文把 hallucination 分解成 detection、source、mitigation 三个角度，说明同一种表面错误可能有不同成因。

## 5. 局限

自动 factuality metric 对开放域和长文本仍不完美；不同模型的拒答风格会影响 hallucination rate；训练数据污染和知识截止时间也可能混淆 source analysis。

**一句话评价**：论文的价值在于把 hallucination 从单一 benchmark 分数扩展成检测-来源-缓解的实证地图，提醒研究者同时报告有效性、谨慎性和事实性。
