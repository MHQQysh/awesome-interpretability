# 178. TriSum: Learning Summarization Ability from Large Language Models with Structured Rationale

- **Authors:** Pengcheng Jiang, Cao Xiao, Zifeng Wang, Parminder Bhatia, Jimeng Sun, Jiawei Han
- **Venue:** NAACL 2024
- **Paper:** https://aclanthology.org/2024.naacl-long.154
- **Tags:** Summarization, Structured Rationale, Distillation, Local Model

## Introduction

LLM 摘要能力强，但规模、传输隐私和部署成本限制了本地应用。直接 distill final summary 丢掉了“哪些 aspect、entity、relation 导出摘要”的结构信息。TriSum 要把大模型能力和 structured rationale 一起蒸馏到小模型。

## Method / Framework

三步流程：LLM 从 document 生成 aspect-triple rationales 与 summary；dual scoring 同时评价 rationale / summary 质量，筛出 golden rationale；小模型使用 curriculum learning，先学结构化 rationale，再学习从 rationale 到 summary 的复杂任务。

## Baselines / Comparisons

比较传统 supervised summarizer、LLM prompting、knowledge distillation、直接 summary distillation 和没有 structured rationale 的 local model。数据包括 CNN/DailyMail、XSum、ClinicalTrial，指标包括 ROUGE、factuality、summary quality 与 rationale interpretability。

## Experiments / Findings

- TriSum 比 baseline 在 CNN/DM、XSum、ClinicalTrial 分别提升约 4.5%、8.5%、7.4%。
- structured rationale 帮助小模型抓住关键 aspect、entity 和 relation，提升长文档和医学摘要的可控性。
- dual scoring 能过滤流畅但不忠实的 teacher output；curriculum 从简单抽取到复杂生成比一次性训练更稳定。
- rationale 让摘要生成过程可检查，用户可看到模型保留了哪些结构事实。

## Ablation / Error Analysis

消融 rationale、dual scoring、golden selection、curriculum 和 rationale / summary joint training。去掉结构化 rationale 后性能和解释性都下降；错误常来自 teacher 抽取漏掉关键关系、结构化表示过长、医学术语和多文档信息融合。

## Limitations

teacher LLM 仍可能 hallucinate，dual scorer 不能保证事实正确；结构化 rationale 的 schema 对领域迁移敏感。小模型只学到 teacher 选择的摘要风格，不能保证遇到分布外文档仍忠实，训练数据构造也有 API 成本。

## 两句话总结

TriSum 不只蒸馏 LLM 的最终摘要，还蒸馏 aspect-triple structured rationale，让本地小模型学会“先找什么、再怎么总结”。多个 benchmark 上的提升说明结构化中间监督有效，但 teacher 错误和 schema 泛化仍是主要限制。
