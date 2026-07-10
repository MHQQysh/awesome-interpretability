# 262. Fostering Appropriate Reliance on Large Language Models: The Role of Explanations, Sources, and Inconsistencies

- **Authors:** Sunnie S. Y. Kim, Jennifer Wortman Vaughan, Q. Vera Liao, Tania Lombrozo, Olga Russakovsky
- **Venue / year:** CHI 2025
- **Paper:** https://arxiv.org/abs/2502.08554
- **Tags:** Human-AI Interaction, Overreliance, Explanation, Sources, Trust

## Introduction

LLM 的回答即使错误也常常流畅、完整、像专家；解释可能帮助用户检查，也可能反过来增加不当信任。论文不是设计一种新的模型解释器，而是问：解释、可点击 sources 和解释内部的不一致，分别如何影响用户对正确/错误答案的 reliance、准确率、信心和行为。

## Method / Framework

Study 1 是 16 人 think-aloud，使用真实交互观察用户如何利用 explanation、source 和 inconsistency 判断可靠性。Study 2 是预注册的 within-subject 2x2x2 controlled experiment，N=308：LLM answer correct/incorrect、explanation absent/present、sources absent/present，参与者完成 8 个二元事实题，每种 response type 一次。论文把 agreement-with-LLM 作为 reliance，把 participant accuracy 作为任务结果，同时测 confidence、source click、time、justification/actionability rating 和 follow-up。

## Baselines / Comparisons

主要“baseline”是八种受控 response condition，而非不同 LLM 模型：neither、explanation only、sources only、explanation+sources，分别在 LLM 正确和错误时比较。Study 1 的开放对话用于发现现象，Study 2 的固定 stimulus 用于隔离因果因素；作者明确不把 explanation 当作 faithful model rationale，而是把它定义为支持答案的自然语言理由。

## Experiments / Findings

- Explanation 会同时提高正确答案的 appropriate reliance 和错误答案的 overreliance。错误回答时，参与者与 LLM 一致率从无 explanation 的 78.2% 升到 explanation-only 的 82.8%，但准确率反而只有 17.2%；正确回答时 explanation-only 准确率为 78.2%，无 explanation 为 67.2%。
- Sources 在没有 explanation 时能区分正确/错误：正确答案一致率 67.2%->73.4%，错误答案一致率 78.2%->68.2%。错误回答准确率 sources-only 为 31.8%，高于 explanation-only 17.2% 和 neither 21.8%；正确回答下 explanation+sources 准确率最高为 79.9%。
- Sources 让用户花更多时间并减少 follow-up；explanation+sources 条件下用户信心和 actionability 评分最高，但这依赖 source 真实、相关、可点击。Study 1 还观察到错误回答中 13 个案例有 9 次用户跟随错误答案，说明 overreliance 不是抽象风险。

## Ablation / Error Analysis

三个因素的交互比单独主效应重要：解释把所有答案都变得更“可接受”，sources 才提供外部检查入口；解释内自相矛盾则成为 unreliability cue，能降低错误答案的 reliance。控制实验牺牲了真实自由对话的噪声，但也意味着 follow-up 没有真正得到第二轮答案；另外 sources 在实验中通常准确，fake/broken/conflicting sources 可能反而制造更强的虚假权威。

## Limitations

参与者任务是二元事实题，不能直接外推到专业决策或长期使用；Study 1 样本很小，Study 2 的 response 是人工控制而非任意 LLM 生成。结果说明用户行为，不证明 explanation faithful，也不代表所有 source UI 或所有文化/专业背景用户都相同。

## 两句话总结

这项 CHI 研究用 think-aloud 与 N=308 的预注册实验拆解了解释、来源和不一致对人类依赖 LLM 的影响。解释会同时放大正确依赖和错误依赖，而真实可核验的 sources 与明显不一致能帮助用户降低 overreliance，因此解释设计必须以 appropriate reliance 而非流畅度为目标。

## Evidence note

本笔记逐段核对 CHI/arXiv PDF 的 Study 1、Study 2、Table 1、交互效应、Discussion 与 Conclusion；条件均按正文表格保留。

