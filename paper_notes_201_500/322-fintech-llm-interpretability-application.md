# 322. Research on the Application Methods of Large Language Model Interpretability in FinTech Scenarios

- **Authors:** Zhiyi Liu, Kai Zhang, Yejie Zheng, Zheng Sun
- **Venue / year:** IEEE CCAI 2024
- **Paper:** https://doi.org/10.1109/ccai61966.2024.10603308
- **Tags:** FinTech, LLM Explainability, Risk Decision, XAI

## Introduction

金融场景同时要求预测能力、审计、风险解释和监管可追责；LLM 的自然语言输出可以改善交互，却可能把不确定的模式包装成确定建议。本文讨论如何将 LLM interpretability 方法应用到 credit/risk/financial decision workflows。

## Method / Framework

公开条目将方法归入 prompt/rationale、feature attribution、retrieval/evidence grounding、human-in-the-loop 和风险监控等组合：模型输出预测后，需要给出输入证据、风险因素、置信度和可审查理由，并保留日志。解释不是单独生成的营销文本，而应连接金融数据、规则和审核流程。

## Baselines / Comparisons

论文适用的比较轴包括传统金融模型+SHAP/LIME、黑盒 LLM、领域微调 LLM、RAG/evidence-grounded LLM 和人工/规则审核。当前工作区没有 IEEE 正文可逐表核对，故不虚构数据集、模型或具体提升。

## Experiments / Findings

公开信息支持的主要结论是：FinTech 解释性必须同时覆盖 model transparency、fairness、risk、privacy 和 accountability；自然语言解释能帮助非技术用户理解，但必须验证事实和合规规则。不同用户（审计员、风险经理、客户）需要不同粒度和形式的 explanation。

## Ablation / Error Analysis

有无 evidence grounding、confidence、SHAP/规则证据、人工审核和不同 prompt 是应检查的消融。LLM 可能 hallucinate financial rationale、泄露敏感数据或产生不公平理由；只评价流畅度/用户满意度会掩盖金融风险。

## Limitations

公开版本不足以确认完整实验协议、监管场景和用户研究，不能将应用建议当作已验证的通用方案。真实金融数据分布、法规变化和高风险错误成本需要外部审计。

## 两句话总结

该工作把 LLM explainability 放入 FinTech 的预测、风险和审计流程，强调证据、置信度、合规规则与人工监督的联合。它提供应用方法论，但具体效果和安全性必须以可访问的正式实验与金融专家验证为准。

## Evidence note

基于 IEEE DOI 题录/公开摘要整理；全文实验细节受限，已明确标注未复核的 baseline 和数字。

