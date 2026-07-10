# 490. Augmented Language Models: A Survey

- **Authors:** Grégoire Mialon et al.
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2302.07842](https://arxiv.org/abs/2302.07842)
- **Tags:** `survey` `reasoning` `tool-use` `interpretability`

## Introduction
传统 LM 受上下文长度、幻觉、知识更新和复杂 reasoning 限制；ALM survey 研究让语言模型调用外部模块、工具、检索器、代码解释器或环境，而仍以 token prediction 为核心。综述特别讨论 augmented behavior 对 interpretability、consistency 和 scalability 的潜在帮助。

## Method / Framework
按两条 augmentation 轴组织：abstract reasoning（CoT、least-to-most、self-consistency、decomposition）与 tool/action use（calculator、code interpreter、search、retrieval、environment）。再按 prompting、demonstration learning、fine-tuning 和 iterative interaction 讨论如何训练模型决定何时调用工具、如何读取结果。

## Baselines / Comparisons
比较纯 LM、CoT/PoT、retrieval-augmented LM、toolformer/ReAct/decision-transformer 类方法，以及显式 symbolic/program execution。表格综合 GSM8K、GSM-HARD、QA 和 tool-use benchmarks 的 accuracy、consistency、context cost 与外部模块调用成本。

## Experiments / Findings
survey 的发展流程从“让模型在文本里写 reasoning”走向“让模型把可验证子问题交给外部工具”，再走向 reasoning+retrieval+action 组合。工具能提供可执行 intermediate state、降低算术/事实错误并增加可审计轨迹，但调用策略、错误工具结果和长链条会产生新的 failure。

## Ablation / Error Analysis
CoT 不一定 faithful，tool call 也可能是错误 query；retrieval context 可能噪声过多，代码执行可能把错误程序执行得很确定。综述总结的关键 ablation 是去掉 tool、改变 reasoning decomposition、使用/不使用 feedback 与不同 retrieval integration。

## Limitations
survey 收录的方法定义和 benchmark 异质，不能把一张表当成统一 meta-analysis。ALM 的自然语言轨迹仍可能是 post-hoc，外部工具提供的是可检查结果而不是模型内部因果解释。

## 两句话总结
ALM 将 reasoning、tool use 和 action 接入标准语言建模，使中间步骤更可执行、更容易核验。它扩展了解释和一致性边界，但也把错误转移到 query、工具选择、retrieval 和多轮控制。

## Evidence note
已读取 survey 的 reasoning/tool/action taxonomy、比较表、retrieval/tool limitations 和 discussion/conclusion。
