# 403. GAMformer: Bridging Tabular Foundation Models and Interpretable Machine Learning

- **Authors:** Andreas Mueller, Julien Siems, Harsha Nori, David Salinas, Arber Zela, Rich Caruana, Frank Hutter
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2410.04560
- **Tags:** `tabular-foundation-model` `GAM` `interpretability` `TabPFN`

## Introduction
Tabular foundation models 如 TabPFN 在小数据分类上很强，但其 prediction 过程难以向领域用户解释。广义加性模型 (GAM) 以每个 feature 的 shape function 贡献预测，透明却可能缺少 foundation model 的表达能力。GAMformer 研究能否把强 tabular Transformer 的性能蒸馏/重构成可加、可视化的模型。

## Method / Framework
GAMformer 将 tabular foundation model 的 feature interactions 与 additive explanation 结合：每个输入 feature 经过 Transformer 表示，再产生可独立检查的 feature contribution/shape；最终 prediction 可分解为各 feature 贡献的和。这样用户可以画出“feature 值变化如何改变 logit/预测”的曲线，而不必解释全部 attention。

## Baselines / Comparisons
对比对象包括 TabPFN 等 tabular foundation models、传统 GAM/Explainable Boosting Machine、树模型和其他可解释/黑箱 tabular 方法。评价同时看预测指标和 fidelity/interpretability：如果 GAMformer 的解释分解不能重构模型输出，就不能把 shape function 视为 faithful explanation。

## Experiments / Findings
论文在多个 tabular benchmark 上显示 GAMformer 试图缩小 TabPFN 的性能与 GAM 的透明性差距。可视化的 feature-wise effects 使用户能检查非线性、阈值和方向；模型输出也可以通过 additive contributions 做局部和全局分析。其价值更偏向 interpretable-by-design，而不是事后近似一个黑箱。

## Ablation / Error Analysis
关键消融是是否加入 feature-wise additive constraint、interaction/Transformer 模块和不同数据规模。纯 additive 可能丢失高阶 feature interactions；加入交互越多越强，但解释复杂度上升。需要检查 shape function 是否受 feature correlation、missing values 和 dataset shift 影响。

## Limitations
表格模型的透明 feature contribution 仍不等于因果关系，相关 feature 会把效应分摊得不稳定。不同数据集的 foundation-model 预训练和调参不完全公平；GAMformer 在强交互任务上可能牺牲准确率。它也不是 LLM，而是清单中的 interpretable foundation-model 邻近工作。

## 两句话总结
GAMformer 尝试把 tabular foundation model 的表示能力转成可加的 feature contribution，使预测可以用 shape function 直接检查和可视化。它提供了结构性解释而非 post-hoc rationale，但 additive 约束、相关特征和交互丢失仍决定性能-透明性折中。

## Evidence note
已读取本地 PDF 的摘要、方法动机、GAM/TabPFN 对比和解释性讨论；本文的详细结果以论文表格为准，笔记不把综述摘要之外的数字写成已核对值。
