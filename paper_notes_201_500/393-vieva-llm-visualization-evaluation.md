# 393. vi(E)va LLM! A Conceptual Stack for Evaluating and Interpreting Generative AI-based Visualizations

- **Authors:** Luca Podo, Muhammad Ishmal, Marco Angelini
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2402.02167
- **Tags:** `LLM` `visualization` `evaluation` `hallucination` `human-in-the-loop`

## Introduction
LLM 可以用自然语言生成 Matplotlib/D3/Vega-Lite 代码或直接生成图像，但“能生成”不等于“图正确”。同一用户意图可以有多个 prompt 和多种可视化实现，模型还可能产生语法错误、错误数据映射、违反 visualization best practices 或语义 hallucination。本文提出的目标是把 LLM-generated visualization 的评测拆成可解释的层级。

## Method / Framework
EvaLLM 是一个 conceptual stack 和 web platform，而不是新的生成模型。它把评测从底到顶分层：

1. **Code layer:** 代码/grammar 是否可执行、结构是否与环境一致；
2. **Representation layer:** 数据字段映射、mark/visual encoding、axis/reference element 是否正确；
3. **Semantic and task/user layers:** 图是否表达用户 query 的意图、是否支持 insight、可读性和人类使用。

平台支持 automatic scoring、manual scoring、多 assessors、细粒度 semantic error 标注和对不同生成系统的 benchmark。输出可以定位是 syntax failure、wrong mapping、poor design 还是 interpretation failure，而不只是一个总分。

## Baselines / Comparisons
相关工作包括 rule-based/NLP visualization recommenders、ncNet、RGVisNet、ChartGPT、AI Thread、GPT/Code Interpreter 和 Llama 系列。本文不提出一个新的 accuracy baseline，而是补充这些生成模型缺少的多层评价协议。两种 case study 比较 GPT-3.5-turbo with Code Interpreter 和 Llama2-70B。

## Experiments / Findings
平台在 NvBench 的 50 个样本上做两个 qualitative use cases。论文发现 GPT-3.5 和 Llama2 都能生成有用图，但错误分布跨层：有些是结构/代码错误，有些是数据字段和视觉编码错误，还有一些是更高层的语义偏离。EvaLLM 的价值是让研究者知道模型失败在哪一层，并将自动分数与人工语义判断放在一起。

文章还指出传统 dataset 有局限：nvBench 许多 utterance 直接写出 chart type/columns，NLVCorpus 只有少量表，合成 query 可能偏离真实用户。由此，模型高分不一定代表处理了开放式、含糊的 visualization intent。

## Ablation / Error Analysis
论文不是参数消融研究，层级拆解本身承担 error analysis：代码可执行但数据映射错误；映射正确但 mark/axis 不合适；视觉结果正确却不满足 query；自动 checker 认为通过但 human assessors 发现语义误导。manual multi-assessor 机制用于发现自动 metrics 漏掉的错误。

## Limitations
EvaLLM 仍是 conceptual stack，层级边界和评分标准需要更多专家验证；两个模型、50 个样本的 case study 不能给出普适排行榜。人工评分成本高且存在一致性问题，自动指标也会依赖 rendering environment、dataset 和 query 明确程度。

## 两句话总结
EvaLLM 将 LLM 生成可视化按代码、表示、语义和用户任务分层评估，使 hallucination 和 visualization failure 可以定位而非被一个总分掩盖。它的贡献是可解释评测协议和平台，不是证明 GPT-3.5 或 Llama2 在开放式可视化中可靠。

## Evidence note
已读取 arXiv 2402.02167 本地 PDF 的摘要、相关工作、EvaLLM stack 和两项 50-sample case study；没有将概念框架误写成统一 benchmark。
