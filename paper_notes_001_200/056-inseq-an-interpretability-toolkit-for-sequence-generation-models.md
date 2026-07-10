# 056. Inseq: An Interpretability Toolkit for Sequence Generation Models

- **作者 / venue**：ACL Demo 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-demo.40/)

## Introduction 与方法

过去 attribution 工具主要服务分类，生成模型需要 token-by-token attribution、custom target、intermediate layer 分析和高效 batching。Inseq 提供统一 Python toolkit，支持 causal LM、seq2seq、不同 attribution method 与可视化。

## Baseline 与功能对比

支持 Integrated Gradients、attention-based attribution、discretized IG 等，并与已有 attribution libraries/方法互操作。工具关注的是覆盖模型、目标 token、层和 batch 的工程能力，而非宣称某一种 attribution 永远正确。

## Findings / case studies

- 性别偏见 MT case study：按 occupation token 的 attribution 排名，并与美国劳动力统计计算 Kendall correlation。
- 工具能分析单步 token generation，也能追踪整条 sequence 的贡献。
- custom attribution target 让研究者可以问“哪一层/哪个 token 影响某个生成词”，而不是只看最终句子。

## 局限

Inseq 本身不提供 faithfulness/plausibility 的最终判据；不同 attribution method 可能不一致，用户仍需做 deletion、counterfactual 或 activation intervention。

**一句话评价**：Inseq 解决的是生成式可解释性研究的基础设施问题，让 attribution 实验可重复、可扩展、可组合。
