# 294. From Understanding to Utilization: A Survey on Explainability for Large Language Models

- **Authors:** Haoyan Luo, Lucia Specia
- **Venue / year:** 2024 survey
- **Paper:** https://arxiv.org/abs/2401.12874
- **Tags:** LLM Explainability Survey, Methods, Tasks, Applications

## Introduction

LLM 已进入搜索、教育、医疗和内容生成，但黑盒性让用户难以判断能力边界、偏见和错误。本文的主线从“理解模型为何这样输出”扩展到“如何利用解释改进模型、任务和用户决策”，系统整理 explainability methods 与 LLM downstream tasks。

## Method / Framework

survey 按 explanation target、method、task 和 stakeholder 组织文献：模型内部 feature/attention/activation、输入 attribution、counterfactual/contrastive、natural-language self-explanation、probing、model editing 和 human evaluation。它还讨论 explanation 用于 debugging、bias/hallucination detection、trust/calibration、data/model improvement 与 responsible deployment 的路径。

## Baselines / Comparisons

比较 intrinsic vs post-hoc、local vs global、black-box vs white-box、faithful vs plausible、human-centered vs model-centric explanation，并梳理 QA、classification、generation、reasoning、dialogue 等任务。文章强调不同方法的 access requirement、cost、interpretability granularity 和 evaluation 不可直接互换。

## Experiments / Findings

survey 的总结是 LLM explainability 不能只依赖 attention 或 self-generated rationale；高质量研究需要把 explanation quality、faithfulness、factuality、robustness 和 user usefulness 同时报告。解释的实际价值包括定位 hallucination/bias、帮助模型编辑与控制、提高开发者调试效率，但在高风险应用中必须有独立证据与人类监督。

## Ablation / Error Analysis

常见误区是把 plausible language 当 faithful mechanism，把 probe accuracy 当 model usage，把 trust increase 当正确决策提升。survey 建议使用 perturbation/causal tests、counterfactual consistency、human studies 和 task outcome 做互补验证，并记录 prompt/model/version 以控制可复现性。

## Limitations

文献分类和术语尚未统一，许多 LLM self-explanation 研究缺少严格 faithfulness test；survey 也无法解决闭源模型内部 access 受限和指标不一致。随着 SAE、attribution graph 和 agentic systems 发展，taxonomy 需要持续更新。

## 两句话总结

这篇 survey 将 LLM explainability 从理解方法整理到可用于调试、控制、检测和责任部署的完整流程。它的核心提醒是解释必须按目标和受众评价，并用 faithfulness、稳健性与实际任务结果约束自然语言的说服力。

## Evidence note

本笔记逐段核对 survey 的 methods/tasks taxonomy、evaluation、utilization 和 open challenges；未把综述引用的数字当作新实验。

