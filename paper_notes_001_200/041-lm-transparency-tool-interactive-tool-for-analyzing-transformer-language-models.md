# 041. LM Transparency Tool: Interactive Tool for Analyzing Transformer Language Models

- **作者 / venue**：ACL Demos 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-demos.6/)
- **类型**：开源 Transformer 内部分析工具

## Introduction

研究者需要在同一界面里查看 token attribution、component promotion、attention 和文本模式，但现有工具往往只支持单一解释，或把模型调试、可视化和比较拆成多个脚本。LM Transparency Tool（LM-TT）试图把“从输入追踪到模型行为”的过程工具化。

## Method / 功能

- 输入 token attribution：查看哪些 token 对预测贡献最大。
- component-level analysis：追踪不同 layer、attention head、MLP 或 residual component 对 token promotion 的影响。
- attention map 与 contribution map：区分“谁关注谁”和“谁真正改变输出”。
- 多模型/多样本比较与 error analysis。
- 结合 Transformer 结构，让用户从 token 逐层追溯到组件，而不是只看 post-hoc saliency。

## Baseline 与对比

论文把 LM-TT 与已有 attribution visualization、patching-based analysis 和模型调试工具对比。patching 通常更强但计算昂贵；论文强调其组件追踪流程在某些分析上可达到约百倍级效率优势。工具还把 white-box analysis 与 LLM self-explanation 区分开。

## Findings

LM-TT 的主要 finding 是工程性的：统一界面减少了从发现异常 token 到定位内部组件的切换成本。工具能帮助研究者提出“哪个 head 提升了这个 token”“某个错误是 input attribution 还是 layer interaction 造成的”等问题，但它不自动证明 attribution 是因果的。

## 局限

目前工具依赖 Transformer 架构和具体 model hooks；新模型需要适配。可视化结果仍需用户理解 residual/attention 语义，不能把图直接当作解释结论。

**一句话评价**：LM-TT 把 mechanistic interpretability 的分析操作变成可复用工具，价值在于缩短定位链路，而不是提出新的解释指标。
