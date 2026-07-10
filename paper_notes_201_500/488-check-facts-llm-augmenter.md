# 488. Check Your Facts and Try Again

- **Authors:** Baolin Peng, Michel Galley, Pengcheng He, Hao Cheng, Yujia Xie, Yu Hu, Qiuyuan Huang, Lars Liden, Zhou Yu, Weizhu Chen, Jianfeng Gao
- **Venue / year:** NeurIPS 2023
- **Paper:** [arXiv:2302.12813](https://arxiv.org/abs/2302.12813)
- **Tags:** `external-knowledge` `feedback` `factuality` `LLM-AUGMENTER`

## Introduction
LLM 在 customer service、open-domain QA 和 table QA 中会生成事实错误；单纯提供更多 context 不能保证模型查证。LLM-AUGMENTER 研究如何让模型调用 retriever、知识库和 feedback policy，并根据 factuality/relevance 反复修正答案。

## Method / Framework
系统包含 external knowledge sources、retriever、knowledge selection、answer generator 和 feedback policy；policy 可选择 critique、self-criticism、external feedback，再把 feedback 交给 LLM 重写。知识来源覆盖 Wikipedia、用户/业务数据库和 OTT-QA tables，评价回答质量与证据使用。

## Baselines / Comparisons
比较 vanilla ChatGPT/LLM、retrieval-only、gold knowledge、BM25 和不同 feedback policy；任务含 Topical-Chat/customer service 与 OTT-QA。人工评估 factuality、appropriateness、helpfulness 与 citation/knowledge grounding。

## Experiments / Findings
LLM-AUGMENTER 在自动和人工评估上明显优于只生成或只检索的方法，摘要报告在相关任务上约提升 10 和 6 个百分点。知识证据与反馈结合能降低 hallucination，并使模型在多轮 refinement 中逐步改正；gold knowledge 上限高，但现实 retriever 错误会传递到回答。

## Ablation / Error Analysis
不同 feedback（external critic、self-criticism、knowledge selection）和 policy 的 ablation 显示，所有反馈都增加 prompt/latency，某些 self-criticism 可能只是重复模型偏见。去掉检索、知识选择或 feedback 会分别损害 factuality/utility，说明解释/证据模块需联合优化。

## Limitations
多次调用、retrieval 和 verification 增加成本与延迟；自动 evaluator 与 LLM judge 不能完全替代事实核验。结果依赖知识源覆盖，真实开放域的 conflicting evidence、长文档和 adversarial knowledge 仍是挑战。

## 两句话总结
LLM-AUGMENTER 把外部知识、检索和反馈循环组合起来，让模型在生成后检查并修正事实。它显著改善 factuality，但每一层 retriever/critic/policy 都可能出错，且 grounding 带来额外成本。

## Evidence note
已读取 LLM-AUGMENTER 架构、知识源、自动/人工评估、policy/feedback ablation 和现实部署代价。
