# 065. Evaluating Open-Domain Question Answering in the Era of Large Language Models

- **作者 / venue**：ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-long.307/)

## Introduction / 问题

开放域 QA 常用 lexical matching 判断答案是否正确，但 plausible answer 不一定与 gold string 相同，尤其是 LLM 生成的释义、别名和完整句。论文重新审视 evaluation，比较 lexical、retrieval、reader 和 generative model 的评测可靠性。

## Baseline

包含 LCC、PRIS、Exact Answer、UWMT 等传统指标，以及 BM25、ANCE+、GAR+、R2-D2 等 retrieval/reader 系统。核心比较是 lexical match 是否低估了正确答案。

## Findings

严格字符串匹配会把语义正确但表面不同的答案判错，也可能把包含答案词但事实错误的回答判对。LLM QA 评测需要结合 semantic equivalence、evidence grounding 和人工核验，不能只报告 EM/F1。

## 局限

开放域答案的事实性和可接受变体难以穷举；LLM judge 也可能继承 prompt/知识偏差。评测协议必须同时记录检索证据和生成答案。

**一句话评价**：论文的可解释性贡献是把“答案为什么被判对/错”从 lexical overlap 重新拉回语义和证据层面。
