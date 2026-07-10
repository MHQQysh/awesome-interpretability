# 355. Knowledge Graphs as Context Sources for LLM-Based Explanations of Learning Recommendations

- **Authors:** Hasan A. Rasheed, Christian Weber, Madjid Fathi
- **Venue / year:** IEEE Global Engineering Education Conference (EDUCON), 2024
- **Paper:** https://osf.io/rvnx3/download
- **Tags:** `knowledge-graph` `rag` `education` `recommendation-explanation` `hallucination`

## Introduction
Learning recommendations need explanations that tell a learner not only what was recommended, but why the item belongs to the learning path and how it supports the goal. GPT-style generation is fluent but can add irrelevant or imprecise claims, which is particularly risky in education. The paper asks whether a curated educational knowledge graph can supply factual context while preserving the LLM's ability to phrase a readable explanation.

The central design choice is to keep the pedagogical structure outside the LLM. Domain experts define the learning taxonomy and explanation template, while GPT-4 fills controlled information slots using graph-derived context. This is a grounded explanation pipeline rather than an attempt to interpret GPT-4's internal parameters.

## Method / Framework
The KG has four levels: learning goals, courses, topics, and open educational resources. A recommendation algorithm based on graph exploration and path weighting identifies an optimal learning path using an MDP-style procedure. For each recommended learning object, the system extracts four kinds of context: its position in the hierarchy, semantic relations to similar learning objects, its graph community, and supporting metadata from connected objects.

These relations are converted to text and inserted into a GPT-4-1106-preview prompt. The prompt has a task body, a role such as teacher, terminology definitions, supporting KG content, and pedagogical instructions supplied by experts. GPT-4 fills a fixed explanation template, and a constrained chat interaction lets a learner ask why an item was recommended or how it relates to the goal; the first response is followed by a confirmation step to reduce drift.

## Baselines / Comparisons
The primary comparison is GPT-4 with KG-based contextualization versus GPT-4 without the KG context, with output length fixed so that ROUGE overlap is comparable. Human-written explanations from learning-object metadata act as the reference, not as a claim that one wording is uniquely correct.

The qualitative comparison is between contextualized and non-contextualized explanations reviewed by domain experts and learners. The design also contrasts free-form generation with the expert-designed template, and distinguishes factual/semantic overlap from pedagogical reflection that requires a human mentor.

## Experiments / Findings
The reference set contains 52 human-generated explanation samples from 10 learning-path recommendations. For every reference, the authors generate one contextualized and one non-contextualized GPT-4 explanation and compute ROUGE-1, ROUGE-2, ROUGE-L, and ROUGE-Lsum precision, recall, and F1. Figure 3 shows that KG context improves precision, recall, and F1; recall is strongest with ROUGE-Lsum, while precision and F1 are better with ROUGE-1.

The qualitative study includes eight learners and five domain experts. Contextualized explanations receive a mean quality score of 4.7/5 and are judged to contain less irrelevant text. Experts report that all automatic samples were not wrong or misleading in the study, but also note that phrasing changes meaning and that a useful explanation should connect the content to the learner's daily context, a reflective step beyond the LLM's available information.

## Ablation / Error Analysis
The main ablation removes KG context while holding generated length constant. The gain is not simply more text: the graph narrows the answer to the recommended object's curriculum location, semantic neighbors, communities, and metadata. The expert prompt/template is another control point because it defines which pedagogical slots the LLM must fill.

The ROUGE analysis cannot judge sentence-level pedagogical correctness or whether a phrasing is appropriate for a particular learner. Experts specifically flag this as an evaluation gap. The small sample also makes the result a proof of concept, and a fluent explanation can still fail to provide the high-level reflection needed for transfer to the learner's real situation.

## Limitations
The experiment is small and uses GPT-4 as the only tested LLM, so cross-model behavior is unknown. ROUGE rewards word-pattern overlap and does not fully measure factual grounding, causal usefulness, or pedagogical quality. User-specific data are deliberately excluded for GDPR reasons, and the authors identify local models and stronger personalization as future work.

## 两句话总结
这篇 EDUCON 工作用教育知识图谱中的课程层级、语义关系、社区和元数据来约束 GPT-4，再由专家设计的模板把这些事实填成学习推荐解释。相对无 KG 上下文的 GPT-4，ROUGE 和用户接受度都提升，但学习者所需的个性化反思和措辞判断仍然需要教师或领域专家。

## Evidence note
本笔记依据 EDUCON 2024 全文逐节核对；ROUGE 数值以论文 Figure 3 展示的相对结论为准，原文未在正文中给出每个柱子的完整可复制数表。
