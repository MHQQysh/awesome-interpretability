# 256. Enhancing AI Decision-Making with Explainable Large Language Models (LLMs) in Critical Applications

- **Authors:** Santosh Kumar, RVS Praveen, RaviTeja Aida, Neeraj Varshney, Zaid Alsalami, Nandini Shirish Boob
- **Venue / year:** IEEE ACROSET 2025
- **Paper:** https://doi.org/10.1109/acroset66531.2025.11280656
- **Tags:** Explainable LLM, Critical Applications, Decision Support, XAI

## Introduction

论文关注医疗、金融、工业等 critical applications 中的决策支持：LLM 的自然语言能力提高了交互性，但黑盒输出难以审计，错误解释还可能让使用者过度信任。它要解决的问题是如何把 LLM 的预测、证据、理由和人类监督组织成更透明的 decision-making pipeline，而不是只把“请解释一下”附加在答案后面。

## Method / Framework

公开可获得的条目与摘要将方法描述为 explainable LLM decision-support framework，重点包括上下文整合、自然语言 rationale、可追溯证据和 human-in-the-loop 验证。其解释目标包含 prediction justification、风险/不确定性提示和面向领域用户的可读输出，强调在部署前进行 validation、auditability 与伦理约束。

## Baselines / Comparisons

该条目在当前工作区没有可直接读取的开放 PDF 正文，公开记录主要是 IEEE 元数据与摘要。因此这里不伪造具体 baseline、数据集、表格或百分点；可合理归类的比较轴包括 opaque LLM、传统 XAI/post-hoc explanation、领域模型与人工/规则决策支持，但这些是阅读框架，不是论文已经报告的数字结果。

## Experiments / Findings

从公开摘要可确认论文主张：在关键应用中，解释性 LLM 可以增强决策透明度、用户理解和信任，并需要结合领域知识、事实依据和 human oversight。由于没有获得正文实验协议，无法确认其是否做了受控用户实验、哪类模型、怎样定义 explanation faithfulness，也不能把“提升”写成已核实的数值结论。

## Ablation / Error Analysis

可复核的误差风险主要是 hallucinated rationale、证据与结论不一致、敏感领域偏见以及用户把流畅解释误当成真实因果。真正需要核对的消融应包括有无 retrieval/evidence grounding、有无 uncertainty display、有无人审和不同模型规模；公开版本不足以证明这些对照已被完整实施。

## Limitations

本文笔记明确区分“论文公开主张”和“正文已核对结果”：当前只拿到题录、摘要级材料，不能声称完成了实验表格级复核。IEEE 论文的具体方法、数据和评估指标需要补充可访问全文后再更新；在此之前，本条目的人工精度只能达到保守证据边界。

## 两句话总结

这篇工作把可解释 LLM 放进关键领域决策支持流程，强调证据、自然语言理由、风险提示和人类监督的组合。当前公开全文不足，能确认研究动机与设计方向，但不能把摘要中的应用性主张当作已核验的 SOTA 实验结论。

## Evidence note

基于 IEEE DOI/J-GLOBAL 题录与公开摘要整理；全文受限，已显式标注未核对的 baseline 和数值，避免把推测写成论文结果。

