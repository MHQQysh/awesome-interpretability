# 293. Human-Centered Explainable AI (HCXAI): Reloading Explainability in the Era of Large Language Models

- **Authors:** Upol Ehsan, Elizabeth Anne Watkins, Philipp Wintersberger, Carina Manger, Sunnie S. Y. Kim, Niels van Berkel, Andreas Riener, Mark Riedl
- **Venue / year:** CHI 2024
- **Paper:** https://doi.org/10.1145/3613905.3636311
- **Tags:** Human-Centered XAI, HCI, Explanation Design, LLMs

## Introduction

XAI 过去常把解释定义成模型输出 feature importance 或技术指标，却忽略谁需要解释、在什么任务中使用、用户如何质疑和行动。LLM 让自然语言、对话、个性化和多模态解释更容易，但也会制造流畅的伪解释。HCXAI 的问题是如何把 explainability 重新放回用户、社会情境、权力关系和实际决策流程。

## Method / Framework

论文以 human-centered design 组织 HCXAI：先理解 stakeholder goals、mental models、context 和 consequences，再共同设计 explanation content、timing、interaction、uncertainty 与 recourse。LLM 被视为 explanation interface/agent，而非天然可信的解释器；系统需要支持追问、反事实、证据查看、用户纠正和模型/组织责任。

## Baselines / Comparisons

比较 model-centric XAI、user-centered XAI、explanation-as-output 与 explanation-as-interaction；也讨论 post-hoc、intrinsic、natural-language rationale、visual explanation 和 human-AI collaboration。论文不是单一算法 benchmark，评价目标包括 comprehension、calibration、appropriate reliance、agency、fairness 和 accountability。

## Experiments / Findings

核心 finding 是“更像人类语言”不等于“更适合人类使用”：解释应根据 expertise、time pressure、risk 和 decision authority 定制。LLM 可以降低生成和交互成本，帮助把复杂内部信号转成可问答的 explanation，但应结合 source、uncertainty、contestation 和 human oversight，避免把责任转嫁给模型。

## Ablation / Error Analysis

静态解释 vs 对话解释、短理由 vs evidence-rich explanation、统一输出 vs user-adaptive explanation 都可能影响 reliance；若没有用户纠正/反事实，系统会把 plausible rationale 固化成权威。评价只看 trust 或 satisfaction 会奖励过度说服，必须同时测准确决策、错误发现、可控性和弱势群体体验。

## Limitations

HCXAI 是设计/研究议程，不提供跨场景统一指标或单一实现；人类中心也不自动解决内部 faithfulness、隐私和组织责任。LLM 时代的交互成本、模型更新和多方利益冲突需要长期实地研究。

## 两句话总结

HCXAI 把解释从“模型给出一段理由”重构为面向真实用户、任务和责任链的互动设计。它提醒 LLM 解释系统必须支持证据、质疑、不确定性与纠错，而不能用流畅度替代可依赖性和问责。

## Evidence note

本笔记核对 CHI 论文公开题录/摘要和 HCXAI 议程；综述/立场性结论未改写为算法实验结果。

