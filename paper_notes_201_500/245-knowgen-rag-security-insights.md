# 245. Enhancing Security Insights with KnowGen-RAG: Combining Knowledge Graphs, LLMs, and Multimodal Interpretability

- **Authors:** Paper metadata lists a 2025 ACM security contribution
- **Venue:** ACM 2025
- **Paper:** https://doi.org/10.1145/3716815.3729012
- **Tags:** Security, Knowledge Graph, RAG, Multimodal Interpretability

## Introduction

安全分析既要整合不断更新的威胁知识，又要解释文本、图像等多模态证据；纯 LLM 容易 hallucinate，纯检索又缺少跨实体推理。KnowGen-RAG 的目标是把 knowledge graph、LLM generation 和 multimodal interpretability 组合成可追溯的安全洞察流程。

## Method / Framework

系统以安全知识图为结构化外部 memory，检索实体/关系路径后交给 LLM 生成分析与建议；多模态输入先做实体/事件抽取，解释层把结论连接回证据、图节点和关系。相比只拼接文本 RAG，graph structure 用于保留攻击者、技术、事件和因果关系。

## Baselines / Comparisons

应比较 closed-book LLM、vanilla text RAG、KG-only retrieval 和 multimodal fusion，并从 security insight accuracy、evidence grounding、explanation completeness、hallucination 与 analyst usefulness 评价。重点是可追溯证据链，而非只看生成流畅度。

## Experiments / Findings

- KG 约束能减少安全实体和关系混淆，并把输出连接到外部证据；多模态解释帮助 analyst 理解图像/文本线索如何支持判断。
- 主要收益来自 structured retrieval 与 evidence-grounded generation 的组合，而不是单独扩大 LLM；系统适合 threat intelligence、attack technique mapping 等需要最新知识的场景。
- 由于该条目公开可复现全文有限，具体数据集、基线和数值应以正式 ACM 版本进一步核验，不在此将检索式摘要当作 SOTA 数字。

## Ablation / Error Analysis

关键消融应包括 KG、multimodal evidence、path retrieval、LLM explanation 和 evidence citation；缺少实体链接或图覆盖时会产生 false grounding。知识库过时、图边错误和 LLM 过度推理是安全场景的高风险错误。

## Limitations

安全知识图的维护成本、敏感数据隐私和攻击者投毒会影响可靠性；解释有证据链接不等于每条因果结论真实。由于正式全文访问受限，本笔记明确保守，不替代对作者实验表格的人工复核。

## 两句话总结

KnowGen-RAG 将安全知识图、多模态证据和 LLM 生成结合，目标是得到可追溯、可解释的威胁洞察而非无依据的文本。其关键风险在图覆盖、实体链接、知识时效和安全证据投毒，具体数值需以正式全文复核。
