# 176. TofuEval: Evaluating Hallucinations of LLMs on Topic-Focused Dialogue Summarization

- **Authors:** see paper
- **Venue:** NAACL 2024
- **Paper:** https://aclanthology.org/2024.naacl-long.251
- **Tags:** Dialogue Summarization, Hallucination, Evaluation, Topic Focus

## Introduction

对话摘要不仅要忠实，还要围绕指定 topic；普通摘要指标可能奖励流畅但引入新事实的文本。TofuEval 构建 topic-focused dialogue summarization 评测，专门区分 intrinsic contradiction、extrinsic unsupported content 和 topic relevance。

## Method / Framework

数据与评测要求模型从多轮对话生成指定主题摘要，再用 claim-level / sentence-level verification 检查每个陈述是否被对话支持。TofuEval 结合自动 NLI、LLM judge 和人工分析，并报告 hallucination rate、faithfulness、topic focus 与 summary quality。

## Baselines / Comparisons

比较传统 fine-tuned summarizer、prompted LLM、不同 instruction / CoT、closed-book 与 dialogue-grounded 方法。指标包括 ROUGE / BERTScore、factual consistency、topic coverage、hallucination precision / recall。

## Experiments / Findings

- LLM 生成摘要通常比传统模型流畅，但在对话中的隐含关系、说话者和数字细节上容易 hallucinate。
- topic instruction 能提高相关性，却可能让模型为补全主题而生成对话没有支持的内容。
- claim-level verification 比整段 binary score 更容易定位错误；LLM judge 在解释方面较强，但需要与 NLI / 人工结合。
- TofuEval 揭示 ROUGE 高不等于 faithful，topic focus 与 factuality 之间存在 trade-off。

## Ablation / Error Analysis

消融 topic prompt、对话长度、speaker information、claim segmentation 和 judge。错误集中在跨轮共指、否定、说话者归属和把常识补充成对话事实；过长对话还造成检索或上下文截断。

## Limitations

topic-focused benchmark 主要是英文和特定领域，对真实客服、会议和多语言对话的覆盖有限。自动 NLI / LLM judge 可能误判隐含事实，人工标注成本仍高；topic relevance 的定义也依赖 rubric。

## 两句话总结

TofuEval 把对话摘要 hallucination 放到 topic-focused 场景中，显示相关性、流畅度和忠实性不能由一个 ROUGE 分数代表。claim-level 检查能提供更可操作的错误定位，但说话者、共指和长上下文仍是难点。
