# 479. Retrieval-Augmented Generation for Large Language Models: A Survey

- **Authors:** Yunfan Gao et al.
- **Venue / year:** Survey 2023
- **Paper:** [arXiv:2312.10997](https://arxiv.org/abs/2312.10997)
- **Tags:** `RAG` `hallucination` `retrieval` `evaluation-survey`

## Introduction
LLM 的参数知识会过时、缺少领域细节并产生 hallucination，RAG 用外部知识库在推理时补充证据。论文要建立一套从 Naive RAG 到 Advanced/Modular RAG 的统一演化图，说明 retrieval、generation、augmentation 三个部件如何共同决定 factuality 和可解释性。

## Method / Framework
survey 按 indexing/data structure、query transformation、retrieval、reranking、context compression、generation 和 feedback 组织技术。Naive RAG 是 retrieve-then-read；Advanced RAG 改进 chunk、query、reranker 与 context；Modular RAG 可重排模块、迭代检索、生成式检索、memory/tool/agent 和 fine-tuning。文章还整理下游任务、数据集、benchmark 与 evaluation。

## Baselines / Comparisons
不是单一新模型实验，而是比较 lexical/dense/hybrid retrieval、BM25/DPR/LLM-generated context、single/iterative retrieval、long/short context、RAG 与 parametric-only LLM。评估维度包括 answer accuracy、context relevance、faithfulness、generation quality、noise/counterfactual robustness 和 information integration。

## Experiments / Findings
survey 的发展流程是：早期通过简单相似度取文档，随后优化 chunk/query/reranking，再将 context compression、graph/table/multimodal data、feedback 和 fine-tuning 纳入模块化管线。核心 finding 是检索正确不等于回答忠实，过多冗余 context 可能干扰生成，真正需要同时评估 context relevance、answer faithfulness 和 noise robustness。

## Ablation / Error Analysis
论文总结的典型失败包括 query 与 chunk 不匹配、长上下文截断、表格/层级文档被切碎、retrieved noise、知识库时效性和模型忽略 evidence。生成式 retrieval、迭代检索和 context compression 可以缓解，但会增加成本、循环错误和评估难度。

## Limitations
作为 survey，章节中的方法和 benchmark 异质，不能从综述表格直接推出统一 SOTA。RAG evaluation 仍缺少跨领域标准，faithfulness 常用 judge/LLM 或 answer accuracy 近似，未必是因果证据利用。

## 两句话总结
这篇 survey 把 RAG 从简单检索增强整理成 retrieval、augmentation、generation 和 evaluation 的完整技术谱系。它对可解释性最重要的提醒是：引用了 context 不等于使用了证据，必须同时测相关性、忠实性、噪声鲁棒性与答案质量。

## Evidence note
已读取 survey 的演化分类、三阶段框架、模块技术、评价指标和主要 failure/challenge sections。
