# 153. Answering Questions by Meta-Reasoning over Multiple Chains of Thought

- **Authors:** Ori Yoran, Tomer Wolfson, Ben Bogin, Uri Katz, Daniel Deutch, Jonathan Berant
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.364
- **Tags:** Multi-Chain Reasoning, Self-Consistency, Evidence, Explainability

## Introduction

Self-consistency 采样多条 CoT，只对最终答案投票，丢掉了中间问题、证据和相互补充的事实。多数答案还可能在多个候选都错误时形成错误共识，也无法给出一条统一、可核验的解释。

## Method / Framework

MCR 先为同一问题生成多条 reasoning chains。每条 chain 由 intermediate question、retrieved evidence、intermediate answer 三元组构成；然后把多条 chain 的中间问答合并成 context，交给另一个 meta-reasoner。meta-reasoner 不直接数票，而是筛选相关事实、纠正冲突、重新组织 step-by-step reasoning，并输出最终答案和解释。

chain 生成交错使用 decomposition model 和 retriever；实验比较把 evidence 插入 chain 的不同位置，并测试 code-davinci-002 与 Vicuna-13B。

## Baselines / Comparisons

比较 Self-Ask、普通 CoT、Self-Consistency@3/@5、Self-Consistency with retrieval、SCR（先筛 chain 再投票）、Oracle，以及 MCR / MCR+SC。数据包括 HotpotQA、2WikiMQA、Bamboogle、FEVEROUS、StrategyQA、Fermi 和 Quartz，共 7 个多跳开放域 QA benchmark。

## Experiments / Findings

- MCR 在 7 个 benchmark、两种 LLM / retriever 设置上持续超过已有方法；它利用错误 chain 中仍有用的 intermediate facts，而不是把整条 chain 丢弃。
- 在示例中，三条 chain 的最终答案多数为 No，但其中一条含有关键 seismology fact；MCR 综合中间事实得到正确 Yes，展示了“meta-reasoning 超过 majority vote”的具体机制。
- MCR 的 explanation 由选中的 evidence facts 构成，因此比 Self-Consistency 的答案列表更易人工核验。
- 对同样 chain 数，MCR 通常优于 SC；当 chain 数增大时，MCR 受益于更多互补证据，但 context 过长后收益递减。
- 在 FEVEROUS 等包含表格和事实验证的任务中，显式 evidence chain 能减少只依赖语言先验的错误。

## Ablation / Error Analysis

消融 chain 数、meta-reasoner 是否使用 evidence、question-answer pairs 与 question-evidence pairs、evidence 插入顺序、retriever 和 MCR+SC。把 evidence 先放入 chain 前部比完全交错更稳定；meta-reasoner 仍可能选择错误或过时事实，尤其当多个 chain 都重复同一错误。推理链越长，信息压缩和上下文冲突越容易导致元推理错误。

## Limitations

MCR 需要多次生成 chain 和一次额外 meta-reasoning，推理成本显著高于单次 CoT。最终 explanation 是模型从 chain 中选择的证据，不保证每一步都是内部真实因果过程；retriever 错误会污染所有候选。任务主要是英文开放域 QA，不能直接推广到长对话或工具执行。

## 两句话总结

MCR 把多条 CoT 当作可交叉检查的证据池，让模型对中间事实做元推理而不是只对最终答案投票。它在七个多跳 QA 上提高准确率和解释性，但额外推理成本、检索错误和长上下文冲突仍限制规模化使用。
