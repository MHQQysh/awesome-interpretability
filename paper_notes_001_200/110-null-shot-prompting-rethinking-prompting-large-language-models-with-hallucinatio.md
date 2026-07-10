# 110. Null-Shot Prompting: Rethinking Prompting Large Language Models With Hallucination

> 人工精读笔记：EMNLP 2024。本文反直觉地把 hallucination 写进 prompt，测试它是否能激发创造性、任务性能和 hallucination detection。

## 0. 论文信息

- **作者**：Pittawat Taveekitworachai 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.740)
- **方法**：Null-shot prompting
- **模型/任务**：PaLM 2、Gemini 1.0 Pro、GPT-3.5/4 Turbo、Claude 3 Opus；commonsense、reading comprehension、NLI、TriviaQA、MATH、HaluEval

## 1. Introduction：要解决什么问题

通常 prompt engineering 把 hallucination 视为要消除的坏行为，但作者观察到，在某些创造性、数学或 NLI 任务中，提示模型“look and utilize hallucination”反而可能改变它的搜索和组合策略。论文因此不主张 hallucination 总是有益，而是研究这种 prompt 是否能作为可控的 performance/behavior intervention。

关键问题有三层：null-shot 相对 zero-shot 是否提升任务分数？它是否也提高 hallucination detection？加入 reasoning/CoT 后，hallucination phrase 的效果是否仍然存在？

## 2. Method / Framework：怎么解决

Null-shot prompt 在任务说明中显式承认/利用 hallucination，例如要求模型寻找、使用或从“imaginary section”中利用 hallucinated information。作者拆分 phrase 位置和组件：look、utilize、hallucination 等，并与 zero-shot、zero-shot CoT 和无 hallucination 的 reasoning prompt 对比。

实验跨多个模型和 benchmark，报告相对 zero-shot 的变化并做统计显著性测试；部分结果也比较 text/chat generation 模型以及不同模型训练阶段。

## 3. Baseline / 对比基线

- **Zero-shot**：无额外 hallucination phrase。
- **Zero-shot CoT**：要求逐步推理，作为已知的 hallucination mitigation/accuracy baseline。
- **Null-shot + CoT**：同时加入 reasoning 与 hallucination。
- **Phrase ablation**：只用 look、只用 utilize、不同位置。
- **多模型/任务**：识别效果依赖模型家族和任务类型，而不是把一个模型当普适结论。

## 4. Experiments / Findings：结果如何读

Null-shot 对 reading comprehension、NLI 和部分 arithmetic/MATH 任务有明显提升；论文报告某些模型最高相对 baseline 增幅约 44.62%。PaLM 2 Chat 在 MATH 某些主题如 algebra 获得较大收益，GPT-4 Turbo 在 RACE-m/RACE-h reading comprehension 达到很强结果。

但收益不是普遍的：commonsense reasoning 和 closed-book TriviaQA 经常无提升甚至变化不大，后者需要从参数记忆中准确 recall，而 hallucination phrase 可能鼓励非事实扩展。Claude 3/GPT-4 等更强模型多数任务收益变小，仅 reading comprehension 仍明显，说明模型训练阶段和已有 instruction 对 prompt 的敏感性很重要。

在 HaluEval 上，null-shot 有时反而提升 hallucination detection performance，这是反直觉结果：提示中承认 hallucination 不必然让模型更愿意生成错误，也可能让它更主动检查/区分幻觉。作者同时发现结果依赖原始 zero-shot baseline，低基线模型的相对提升不能直接比较。

## 5. Ablation / 机制解释

- phrase 位置：通常放在 prompt 开头/任务指令附近最有效，位置变化会明显改变结果。
- phrase 组件：完整 phrase 同时包含 look 与 utilize 时往往最好，删掉组件会下降。
- null-shot vs null-shot+CoT：CoT 并不总是叠加收益；某些 MATH 任务 CoT 没有提升。
- chat/text 模型：chat tuning 更可能接受创造性 hallucination 指令，但不同模型差异很大。

## 6. Limitations / 局限与复现注意

- 结果高度依赖 proprietary model、prompt wording、任务和相对 baseline，不能当作通用 hallucination mitigation。
- “hallucination”在创造性任务中可能是有意的非事实生成，和安全意义的 factual hallucination 不同。
- 绝大多数实验是 prompt-level observation，没有内部机制或因果 intervention 解释为什么有效。
- 某些收益可能来自提高探索性/接受非标准答案，而不是可靠性提升。

## 7. 两句话总结

Null-shot prompting 反直觉地在 prompt 中显式提到并利用 hallucination，在 reading comprehension、NLI 和部分数学任务上可显著提升性能，最高报告约 44.62% 的相对增益。它也可能提升 hallucination detection，但对 commonsense、closed-book QA 和强模型不稳定，说明“创造性 hallucination”与“事实性幻觉”必须分开评估。
