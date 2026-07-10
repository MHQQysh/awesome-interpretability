# 462. Explaining Legal Concepts with Augmented Large Language Models (GPT-4)

- **Authors:** Jaromir Savelka, Kevin D. Ashley, Morgan A. Gray, Hannes Westermann, Huihui Xu
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2306.09525](https://arxiv.org/abs/2306.09525)
- **Tags:** `legal-llm` `retrieval-augmented-explanation` `hallucination` `human-evaluation`

## Introduction
法律条文中的 open-textured term 必须结合过往判例解释，但 GPT-4 直接生成的法律说明可能凭流畅语言掩盖错误引用和虚构判例。论文问：加入法律信息检索，从判例中取 high-value explanatory sentences，能否提高 explanation 的 factuality、clarity、relevance、richness 和 on-pointedness。

## Method / Framework
语料含 42 个美国法条短语、26,959 条判例句，其中 1,853 条被人工标为 high value。baseline 只给 GPT-4 条文和术语；augmented system 用 legal IR 检索相关 high-value sentences，再让 GPT-4 生成一句短解释和约十句长解释。两名法律学者对 84 对输出比较五项质量指标并核验每个 case citation。

## Baselines / Comparisons
核心对照是 direct GPT-4 与 retrieved-context GPT-4，而不是与小模型比较。实验同时按 short/long explanation 比较，并区分事实、清晰、相关、信息丰富和是否紧扣术语。

## Experiments / Findings
增强版本在短、长解释的大多数维度都更受偏好；baseline 在 clarity 偶尔更好，但 factuality 上没有一次被偏好。直接 GPT-4 的引用多数存在，却仍出现不存在的 citation、真实 citation 配上虚构内容、无关或错误适用法律；augmented 输出中评审没有发现不存在 citation 或误述所引案件。结果支持 retrieval grounding 能显著压低 hallucination，但前提是检索材料本身正确。

## Ablation / Error Analysis
增强版仍会把州法判例用于联邦术语，引用 dissenting opinion、过时且已被推翻的案件，或把同一术语放到另一法律语境；这些错误来自 IR 组件并会传递给 GPT-4。法律学者也指出 citation formatting 会伤害 clarity，不能把对专家清晰度的结论外推到普通读者。

## Limitations
只有 42 个短语、两名共同作者评审，没有 layperson 或真实法律决策实验。augmented model 并非证明无幻觉，只是在该语料和人工核验范围内未观察到相应 citation 错误；检索质量、时效性和管辖区匹配仍是瓶颈。

## 两句话总结
这篇论文显示 GPT-4 的法律解释表面流畅但可能虚构判例或误述法律含义，加入高质量判例检索能显著提升事实性与相关性。它也明确指出 grounding 不是魔法：错误、过时或不适用的检索证据会被模型放大。

## Evidence note
已逐段读取数据、双系统设计、两名法律专家的短/长解释评审、citation 核验和 IR 误差分析。
