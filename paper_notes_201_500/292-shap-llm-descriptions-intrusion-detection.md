# 292. Enhancing Machine Learning Model Interpretability in Intrusion Detection Systems through SHAP Explanations and LLM-Generated Descriptions

- **Authors:** Abderrazak Khediri, Hamda Slimi, Ayoub Yahiaoui, Makhlouf Derdour, Hakim Bendjenna, Charaf Eddine Ghenai
- **Venue / year:** IEEE PAIS 2024
- **Paper:** https://doi.org/10.1109/pais62114.2024.10541168
- **Tags:** Intrusion Detection, SHAP, LLM Explanation, Cybersecurity

## Introduction

IDS 模型即使能识别攻击，也常把安全分析员留在“为什么报警”的黑盒外面；SHAP 能给 feature contribution，却难以把数值贡献转成领域人员可读的 incident narrative。论文研究将 SHAP explanation 与 LLM-generated description 结合，提升 IDS 决策的可理解性和审计能力。

## Method / Framework

流程先训练/使用入侵检测分类器，对每条流量或事件计算 SHAP feature importance，再把 top positive/negative features、预测类别和上下文交给 LLM，生成自然语言说明。SHAP 负责与模型输出绑定的局部 attribution，LLM 负责语义化、按攻击类型和分析员需求组织理由，二者职责分开。

## Baselines / Comparisons

比较应包括原始 IDS prediction、只展示 SHAP 数值/图、以及 SHAP+LLM 描述；评价重点是 detection performance 是否保持、解释与 SHAP 是否一致、分析员是否更容易理解。公开 DOI 条目未提供当前工作区可逐表核对的全文，因此不伪造具体数据集或 accuracy 数字。

## Experiments / Findings

论文主张 LLM 描述能把诸如 packet/flow feature contribution 转成 security analyst 可读的因果线索，帮助区分 attack type、关键 feature 和报警原因。方法的解释性收益来自 attribution grounding，而不是 LLM 自己“猜原因”；只要 LLM 受限于 SHAP evidence，叙述更容易审计。

## Ablation / Error Analysis

去掉 SHAP 会使 LLM 解释可能脱离 IDS decision；去掉 LLM 则保留忠实数值但降低可读性。SHAP 本身受 feature correlation、background distribution 和模型非线性影响，LLM 还可能把相关性描述成因果，或遗漏负向 feature。需要对 description 与 SHAP top-k、专家标签及反事实重跑共同校验。

## Limitations

网络攻击数据漂移、类别不平衡和隐私会影响 IDS 与解释；LLM 生成文本存在 hallucination、prompt injection 与敏感信息泄露风险。公开材料不足以确认用户研究和跨数据集泛化，不能把可读 narrative 等同于真实安全因果分析。

## 两句话总结

该工作用 SHAP 把 IDS 决策绑定到局部 feature contribution，再用 LLM 把数值证据翻译成安全分析员可读的报警说明。它的价值在于 attribution-grounded explanation，但 SHAP 的相关性和 LLM 的叙事风险仍需专家/反事实验证。

## Evidence note

基于 IEEE DOI 题录与公开摘要整理；全文实验表格未获得，已明确不补写具体指标。

