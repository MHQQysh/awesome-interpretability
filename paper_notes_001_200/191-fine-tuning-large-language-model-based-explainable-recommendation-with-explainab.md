# 191. Fine-Tuning Large Language Model Based Explainable Recommendation with Explainable Quality Reward

- **Authors:** Mengyuan Yang, Mengying Zhu, Yan Wang, Linxun Chen, Yilei Zhao, Xiuyuan Wang, Bing Han, Xiaolin Zheng, Jianwei Yin
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/28777
- **Tags:** Explainable Recommendation, Reward Modeling, Rationale, Personalization

## Introduction

LLM-based explainable recommendation 能生成自然语言，但直接 prompt 或微调会同时暴露三个问题：解释不够个性化、同一物品的解释互相矛盾、训练解释数据本身质量可疑。论文要解决的不是单纯把 BLEU 做高，而是在推荐正确的前提下，让解释与用户偏好和物品特征一致、内容多样且可读。

## Method / Framework

LLM2ER 先从评论构造 concept graph，用异构图预测用户对目标物品的 rating，并沿用户-物品关系路径找出个性化、情感一致的候选 concepts。LLM2ER-EQR 在 backbone 上加入两类 reward：Concept Consistent Reward 用候选 concept 约束解释不要偏离用户偏好，High-Quality Alignment Reward 从已有解释中筛出较高质量样本并推动生成结果对齐，最后用 reinforcement learning 微调生成器。

## Baselines / Comparisons

论文在三个真实推荐数据集上比较 LLM2ER-EQR 与七种 LM-based explainable recommendation 方法，并报告 BLEU-3/4、ROUGE-1/L、Distinct-1/2、Concept Overlapping Ratio (COR) 与 Concept Matching Ratio (CMR)。对照包含只用 backbone 的 LLM2ER，以及去掉 CCR 或 HQAR 的两个消融版本，因此能区分语言表面质量、个性化/多样性和事实/概念一致性。

## Experiments / Findings

- 完整模型在三个数据集上所有主要指标都优于对照；相对最强 baseline，BLEU 和 ROUGE 平均提升约 5.496% 和 4.835%。
- Distinct 指标和 COR 说明解释更个性化、更少复用同一套词；作者报告 Distinct/COR 相对最强对照平均改善约 3.940% / 12.756%，而 CMR 最高，表明生成内容更常覆盖候选用户偏好与物品概念。
- 结果支持“推荐模型的 concept path + LLM generation”这条发展路线：图模型负责可追溯的用户-物品依据，LLM 负责把依据组织成自然语言，而不是把 LLM 当作无条件的解释器。

## Ablation / Error Analysis

去掉任一 reward 都会下降：只有 CCR 时更容易保持概念一致但语言质量和高质量样本对齐不足；只有 HQAR 时文本更像高质量参考，却可能失去目标用户/物品的个性化约束。错误仍来自评论概念抽取、图路径遗漏、LLM hallucination、概念与自然语言短语不完全匹配；COR/CMR 也不是严格的 causal faithfulness 测试。

## Limitations

训练和 reward 仍依赖预训练 LLM 及其隐式知识，可能带来偏见、冒犯性内容、隐私泄漏和幻觉；作者明确指出真实部署还需要后处理。离线文本指标和概念匹配不能完全代表用户是否相信解释，也不能证明解释真实反映推荐模型的内部决策。

## 两句话总结

LLM2ER-EQR 把用户-物品概念图与两种可解释质量 reward 结合，在不可靠解释数据上用 RL 学习更个性化、一致和多样的推荐理由。论文的关键贡献是把“会说话”拆成概念一致性与高质量对齐，但 reward 指标仍不能替代用户研究和因果忠实度验证。
