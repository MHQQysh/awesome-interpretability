# 171. On Behalf of the Stakeholders: Trends in NLP Model Interpretability in the Era of LLMs

- **Authors:** Nitay Calderon, Roi Reichart
- **Venue:** NAACL 2025
- **Paper:** https://aclanthology.org/2025.naacl-long.29
- **Tags:** Survey, Stakeholders, Interpretability Trends, Human-Centered

## Introduction

技术综述常按方法分类，却很少问谁需要解释、为什么需要、要解释什么以及什么形式最有用。作者收集过去十年的 NLP interpretability 论文，并用 LLM 做 relevance filtering / characterization，比较 NLP developer 与非 NLP stakeholder、不同领域和不同解释范式。

## Method / Framework

论文建立 stakeholder-centered taxonomy，按 why（trust、debug、fairness、compliance）、what（input、behavior、internal component、data）和 how（salience、rationale、counterfactual、mechanistic 等）编码论文。趋势分析覆盖 NLP 以及 medicine、psychology、education、economics、neuroscience 等领域。

## Baselines / Comparisons

比较年份、研究领域、解释目标、方法范式和 stakeholder 使用场景，而非比较模型 accuracy。LLM relevance filtering 与人工抽查形成质量控制，内部组件解释和用户可理解解释是重点对照。

## Experiments / Findings

- NLP interpretability 论文数量快速增长，但 NLP 之外的领域更偏向结果、案例和用户可理解解释，很少使用 neuron / internal component explanation。
- developer 与用户对“解释应该回答什么”的需求明显不同；技术上精确的内部机制未必满足医生、教师、决策者或受影响群体的需求。
- LLM 时代研究量增加了，但 stakeholder perspective 仍不足，很多工作默认 explanation 的消费者就是模型开发者。
- 作者据此主张未来评测应同时报告 faithfulness、plausibility、utility、robustness、成本和受众适配，而不是单一 explanation score。

## Ablation / Error Analysis

趋势统计对 relevance filtering、年份区间、领域划分和类别定义敏感；LLM characterization 可能继承标题、摘要和术语偏差。论文用人工抽查和多维编码降低噪声，但不能把论文出现某个方法当成实际用户采用。

## Limitations

文献计量反映发表趋势而非真实部署需求；LLM 自动标注可能漏掉跨领域解释工作。stakeholder 类别和解释质量指标仍需参与式用户研究，不能从论文文本推断所有社会影响。

## 两句话总结

这篇综述把 NLP interpretability 从方法清单改写成 stakeholder 需求问题，显示开发者与外部用户、NLP 与其它领域对解释的偏好存在明显差异。它的主要贡献不是新解释算法，而是提醒研究者把受众、目的、faithfulness 和实际 utility 一起纳入评测。
