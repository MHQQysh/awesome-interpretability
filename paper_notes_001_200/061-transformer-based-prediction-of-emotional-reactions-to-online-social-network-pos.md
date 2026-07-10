# 061. Transformer-based Prediction of Emotional Reactions to Online Social Network Posts

- **作者 / venue**：WASSA 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.wassa-1.31/)

## Introduction / Method

论文预测社交网络帖子在发布后会得到多少不同类型的 reaction，并强调 ante-publication setting：只使用帖子文本和发布前 metadata，不使用评论和回复。模型采用 transformer-based multi-task regressor，再用 SHAP 解释最有影响的 textual features。

## Baseline

比较 Metadata-Only 与 Linear Regression、AdaBoost、Random Forest、MLP 等经典回归器；也比较只用文本和文本+metadata 的设置。

## Findings

Transformer 能从帖子内容预测不同 reaction；SHAP 可以把预测拆回词/短语，帮助用户理解模型为何预期某种情绪反应。结果显示文本比单纯 metadata 更有价值，但时间、主题和平台因素仍影响泛化。

## 局限

预测 reaction 数量不等于理解用户真实情绪；SHAP 是 post-hoc proxy，且社会反应存在时间和平台偏差。

**一句话评价**：这篇工作把情绪预测和可解释回归结合起来，但应把 SHAP 解释与因果影响区分开。
