# 173. ReasoningRec: Bridging Personalized Recommendations and Human-Interpretable Explanations through LLM Reasoning

- **Authors:** Millennium Bismay, Xiangjue Dong, James Caverlee
- **Venue:** Findings of NAACL 2025
- **Paper:** https://aclanthology.org/2025.findings-naacl.454
- **Tags:** Recommendation, Explanation, Synthetic Data, Distillation

## Introduction

传统推荐系统从 click / like 学 implicit preference，通常能预测 item，却无法说明为什么推荐。用户真正需要的是 likes、aversions、主题和风格之间的可读关系；但真实用户很少提供这样的解释，直接监督难以获得。

## Method / Framework

ReasoningRec 先用大 LLM 把用户历史和 item 信息转成偏好、厌恶与 explanation，再用这些 synthetic rationales 微调小 LLM。推理时小模型同时输出推荐和解释，形成从 user-item interaction 到 human-readable reasoning 的桥。

## Baselines / Comparisons

与传统 collaborative / sequential recommendation、LLM prompting、vanilla SLM 和没有 rationale 的 fine-tuning 比较。实验分析 context quality、personalization、reasoning data 和 explanation quality，指标包括 recommendation prediction 与人工 / LLM explanation evaluation。

## Experiments / Findings

- ReasoningRec 的 recommendation prediction 比 SOTA 最多提升约 12.5%，同时生成与用户偏好和 item attributes 对齐的解释。
- contextual / personalized data 质量是关键：粗糙历史会导致 plausible 但不真实的 explanation。
- synthetic rationale 让小模型学习显式 preference reasoning，而不是只拟合 item co-occurrence。
- 解释通常包含主题、情绪、人物和用户 dislike 的对照，因而更接近口头 word-of-mouth recommendation。

## Ablation / Error Analysis

消融 user context、item description、rationale、LLM teacher 和 SLM；去掉 context 时 accuracy 与解释相关性同时下降。错误来自用户历史稀疏、item metadata 不完整、LLM 过度推断和推荐正确但理由不忠实。

## Limitations

teacher-generated rationales 可能是事后合理化，LLM judge 的解释质量不等于真实用户满意度。数据与实验主要是离线 benchmark，尚未验证长期点击、信任和商业影响；生成解释还可能泄露用户敏感偏好。

## 两句话总结

ReasoningRec 用大模型生成用户偏好和 item 关系的解释，再蒸馏到小模型，使推荐预测与可读理由同时提升。它证明 reasoning supervision 有用，但解释的真实性、稀疏历史和长期用户效用仍需在线验证。
