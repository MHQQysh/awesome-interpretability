# 385. Scalable Qualitative Coding with LLMs: Chain-of-Thought Reasoning Matches Human Performance in Some Hermeneutic Tasks

- **Authors:** Zackary Okun Dunivin
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2401.15170
- **Tags:** `LLM` `qualitative-coding` `CoT` `human-evaluation` `rationale`

## Introduction
Qualitative coding/content analysis 将段落文本按 codebook 中的类别标注，过去依赖熟悉领域的研究者逐段解释。难点不是关键词匹配，而是密集段落中的隐含主题、语境和社会历史意义；人工团队规模有限，难以分析大语料。本文研究 GPT-4/3.5 能否在不牺牲人类级 coding fidelity 的情况下扩展这种 hermeneutic task。

作者特别关注 codebook 如何为 LLM 重写，以及让模型给出 rationale 的 chain-of-thought 是否提高和解释 coding 决定，而不是只让模型吐一个 label。

## Method / Framework
研究流程包括：

1. 使用一组社会历史研究中的 dense paragraph passages 和 9 个 codes，建立 human-derived gold standard。
2. 将人类 codebook 改写为适合 LLM 的定义、正例/反例、边界和输出格式，进行 zero-shot coding。
3. 比较 GPT-4 和 GPT-3.5，并比较只输出标签与要求 rationale/CoT 的 prompt。
4. 用 Cohen's kappa 与 human gold/intercoder agreement 比较模型；同时总结 prompt/codebook 设计 best practices。

解释在这里有双重角色：它既是对 coding 判定的可读理由，也可能通过迫使模型展开中间判断来提高 fidelity。但作者没有证明 CoT 文本逐步对应真实内部计算。

## Baselines / Comparisons
主要 baseline 是 human-derived gold standard、human coder agreement、GPT-3.5 和 GPT-4 的不同 prompt。任务不与简单 bag-of-words/传统 classifier 做主对比，因为目标是高语境解释；公平性来自相同 passages、相同 codebook 和相同 zero-shot 条件。

评价使用 Cohen's kappa：约 0.60 视为 substantial，约 0.75 以上视为 excellent。作者还比较 codebook 原始人类版与经过 LLM 任务重写版，以及是否明确要求 reasoning。

## Experiments / Findings
GPT-4 在 9 个 code 中有 8 个达到 substantial agreement (kappa >= 0.6)，其中 3 个达到 excellent agreement (kappa >= 0.79/约 0.75 以上)。GPT-3.5 在相同 prompt 下平均 kappa 只有 0.34，最高约 0.55，明显低于 GPT-4。

加入 rationale/CoT 后 coding fidelity 显著提高，说明让模型把 codebook 条件应用到文本并说明理由，比只要求一个类别更稳定。作者据此认为在部分 codebook 上 GPT-4 已能进行人类级大规模内容分析，研究者可以把时间投入到 codebook 设计、边界案例和更具创造性的工作。

## Ablation / Error Analysis
最关键的消融是 CoT：无理由的 label-only prompt 与要求理由的 prompt 对比，后者在多个 code 上 kappa 提升。另一个 error source 是 codebook 本身：为人类写的模糊定义、类别重叠和缺乏反例会让模型和人类都不稳定；因此作者建议明确 inclusion/exclusion criteria、给出边界例子、避免一个 code 同时承载多个概念。

结果不能解释为 GPT-4 在所有 qualitative coding 上都人类级。code 的难度、文化背景、样本长度、gold 的主观性和污染风险都会改变 kappa；CoT 也可能提高说服力而非真实忠实度，所以仍需盲评、人工复核和跨批次稳定性。

## Limitations
研究是单一社会历史 case study，9 个 codes 不代表全部人文学科。human gold 不是客观真理，kappa 受类别分布和 coder agreement 影响；zero-shot 结果也可能受到训练数据污染。GPT-4/3.5 是闭源版本，模型更新、成本和隐私限制复现。论文没有测试 rationale 是否忠实反映模型内部机制。

## 两句话总结
本文把社会历史文本的 9 类 qualitative coding 交给 GPT-4/3.5，并发现 GPT-4 的 kappa 在 8/9 类达到 substantial、3/9 类达到 excellent，同时 rationale/CoT 能显著提高 coding fidelity。它证明的是特定 codebook 和语料下的可扩展行为一致性，不是模型真正理解或 CoT 忠实揭示内部推理的证明。

## Evidence note
已读取 arXiv 2401.15170 v2 本地 PDF 的摘要、方法、key findings、prompt/codebook 讨论和实验结论；数字按论文摘要和正文表述记录。
