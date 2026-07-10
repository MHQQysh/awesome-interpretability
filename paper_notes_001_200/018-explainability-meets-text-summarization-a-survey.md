# 018. Explainability Meets Text Summarization: A Survey

- **作者 / venue**：Abdul Waheed, Taha Hassan, Pedro M. Ferreira, Iryna Gurevych；INLG 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.inlg-main.49/)
- **任务**：系统综述文本摘要中的 explainability，梳理模型、数据集、解释方式和评价指标

## 1. Introduction：摘要系统为什么需要解释

摘要模型，特别是 LLM，常被当作黑盒：用户看到一个短摘要，却不知道哪些源文档内容被使用、哪些事实被删掉、摘要是否忠实，也不知道模型为什么选择当前组织方式。解释性的对象因此分成两类：面向终端用户解释输出，和面向开发者理解/调试模型。

论文不是提出一个新摘要模型，而是回答五类 survey questions：常用模型/数据集/指标是什么；使用哪些 XAI 技术；解释如何可视化和评价；解释是否提高摘要性能；现有方法对哪些 stakeholder 真正有用。

## 2. Survey Method

- 采用 systematic review 流程，参考 Kitchenham and Charters 的方法。
- 先定义检索、筛选和研究问题，再对摘要解释文献进行归类。
- 分类维度包含：解释对象、解释生成方式、用户目标、评价方法和实际用途。
- 综述还比较 extractive、abstractive、LLM-based summarization 的解释需求差异。

## 3. Baseline / comparison 在综述中的含义

这篇 survey 的“baseline”不是一个单一模型，而是不同 explainability 家族的对比：

- **feature/attention visualization**：展示模型关注了哪些 token，但 attention 不一定等于因果贡献。
- **input attribution / saliency**：用梯度、扰动或重要性分数解释输入影响。
- **rationale / generated explanation**：用自然语言说明摘要为何如此生成。
- **faithfulness evaluation**：删去或保留解释中的证据，检查摘要性能/输出是否变化。
- **practical usefulness**：以人类评价、ROUGE/BERTScore 或任务调试收益判断解释是否有用。

综述强调，readability、plausibility 和 faithfulness 是不同维度，不能用一个漂亮的可视化替代全部评价。

## 4. Findings：领域共识与缺口

- 文献中最常见的是 ROUGE 及其变体，但 ROUGE 主要衡量 n-gram overlap，不能充分评价解释忠实度或事实一致性。
- 解释通常分为输出解释和模型理解解释；前者服务用户信任，后者服务开发者调试，两者的评价协议不应混用。
- 人类评价仍是判断解释可读性和实用性的常见方式，但成本高、主观性强，且评价者可能把“合理”误判为“忠实”。
- LLM 摘要带来新的问题：生成的 rationale 可能只是事后合理化，摘要本身也可能包含 hallucination，因此 explanation faithfulness 和 summary factuality 需要联合评价。
- 综述指出数据集、模型、解释形式和指标缺乏统一协议，导致不同论文之间难以直接比较。

## 5. 对后续研究的启示

摘要解释研究应至少报告：解释对象、用户、生成时间点、faithfulness test、输入删除/保留实验、人工评价协议，以及摘要质量和事实性的独立指标。尤其需要区分“解释帮助用户理解”与“解释揭示了模型真正使用的证据”。

**一句话评价**：这篇 survey 的价值在于把摘要 explainability 从一张 attention 图扩展成完整评价矩阵，并明确指出可读性、忠实度和实际有用性不是同一个指标。
