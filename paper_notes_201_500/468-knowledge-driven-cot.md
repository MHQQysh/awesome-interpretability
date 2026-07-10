# 468. Knowledge-Driven CoT: Exploring Faithful Reasoning in LLMs for Knowledge-intensive Question Answering

- **Authors:** Keheng Wang, Feiyu Duan, Sirui Wang, Peiguang Li, Yunsen Xian, Chuantao Yin, Wenge Rong, Zhang Xiong
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2308.13259](https://arxiv.org/abs/2308.13259)
- **Tags:** `faithful-cot` `knowledge-retrieval` `KBQA` `verifier`

## Introduction
多跳 KBQA 中，LLM 的一个错误 sub-answer 会被后续 CoT 当成事实，造成 error propagation；直接塞入大量百科文本又让模型无法有效阅读。KD-CoT 研究如何让每个中间问题都通过外部 QA 系统核验，从而使 rationale 具有可追踪证据。

## Method / Framework
先把复杂问题转为结构化 multi-round QA：LLM 生成 sub-question，retriever-reader 返回候选答案，verifier 在 LLM answer 与外部答案间选择/修正，再将修正结果反馈给下一轮。作者还用人工 anchor demonstrations + ChatGPT 构造 KBQA CoT collection，以 RoBERTa 相似度选示例，并训练 feedback-augmented DPR (FBA-DPR)。

## Baselines / Comparisons
在 WebQSP 与 ComplexWebQuestions 比较 vanilla CoT ICL、LLM retrieval/QA pairs、structured CoT fixed/selected、去 retrieve-then-read、去 verifier；检索比较 BM25、DPR 和 FBA-DPR。还比较 T5-3B、FlanT5-3B、Llama2-7B-LoRA 的 direct fine-tuning 与 CoT fine-tuning。

## Experiments / Findings
KD-CoT 相对 vanilla CoT ICL 在 WebQSP/CWQ 的 success/Hit@1 绝对提升 8.0/5.1 个百分点；错误问题中 13.5%/10.6% 被交互修正，而仅 5.8%/5.4% 被改错。FBA-DPR 的 WebQSP Hit/Recall@100 为 95.4/88.4，CWQ 为 81.3/76.5，超过对应 BM25/DPR。structured rationale 比直接拼检索段更有效，因为它只把针对当前 sub-question 的知识传给模型。

## Ablation / Error Analysis
去掉 retrieve-then-read 后 WebQSP/CWQ 约降到 66.8/49.4，去 verifier 为 59.9/47.6，完整 KD-CoT 为 68.6/52.5；verifier 并非总是替代 LLM，二者结合最好。WebQSP 多为 single-hop，首轮收益最大；CWQ 更复杂，多轮交互到后续轮仍有收益。CoT fine-tuning 对简单问题略优，复杂多跳反而可能破坏原模型知识。

## Limitations
结构化 ReAct 格式限制 LLM 自由生成 sub-question，格式错误会被系统判为失败；检索库、reader 和 verifier 的错误仍可能污染 rationale。4-shot collection 和 ChatGPT 生成 demonstrations 也引入模型/提示依赖，faithfulness 主要是外部答案一致而非内部因果证明。

## 两句话总结
KD-CoT 把每一步 CoT 变成可检索、可验证、可修正的 QA 操作，减少知识密集型多跳问答的错误传播。它同时提升答案成功率和 retrieval recall，但“有外部证据的 rationale”仍不等于模型内部真正按该证据决策。

## Evidence note
已读取框架图、CoT collection、WebQSP/CWQ 主表、FBA-DPR、去 retriever/verifier、迭代来源分析和 fine-tuning 对照。
