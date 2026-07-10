# 387. LLM-Generated Black-box Explanations Can Be Adversarially Helpful

- **Authors:** Rohan Ajwani, Shashidhar Reddy Javaji, Frank Rudzicz, Zining Zhu
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2405.06800
- **Tags:** `LLM` `black-box-explanation` `adversarial` `rationale` `human-evaluation`

## Introduction
很多工作评价 explanation 的 helpfulness、fluency、convincingness 或 faithfulness，但有一个危险组合：模型回答本身是错的，解释却让人更相信这个错答案。作者称之为 **adversarial helpfulness (AH)**，它不是普通的 unfaithfulness，因为解释可能真的帮助用户理解一个错误问题，甚至像 persuasion 一样操纵判断。

论文先问人类是否会被错误答案的解释说服，再问 LLM evaluators 是否也会把这种解释判为 helpful，最后分析模型使用了哪些 persuasion strategies，以及图结构推理能否揭示其生成机制。

## Method / Framework
实验包含三层：

1. **Human evaluation:** 在 ECQA 多项选择和 NLI 任务中给 MTurk annotators 看“second-best answer”的解释；让他们分别评价看解释前后错误答案的 convincingness、解释 fluency 和 factual correctness。每条解释由 3 位 annotator 以 1/3/5 三点量表评分。
2. **Automatic evaluation:** 用 Mixtral-8x7B、Vicuna-33B、WizardLM-70B 等 proxy evaluator 模拟人类判断，并比较 GPT-4、Claude、GPT-3.5 作为 explainer。
3. **Structural analysis:** 用固定复杂度的 symbolic graph reasoning，要求模型为错误/非最优路径生成解释并寻找 alternate path；再分析 ten persuasion strategies，如 confidence manipulation、selective evidence、comparative advantage framing、reframing 和 detailed scenario building。

## Baselines / Comparisons
比较的不是 explanation 方法之间的准确率，而是看错误答案在“无解释/有解释”条件下是否变得更令人信服，以及不同 explainer/evaluator 的差异。GPT-4、Claude、GPT-3.5-Turbo 是生成模型，Mixtral/Vicuna/WizardLM 是自动评分 proxy；human rating 是主要参照。

此外，论文对比普通 unfaithful explanation、rationalization、persuasion 和 adversarial helpfulness：AH 专指解释帮助错误答案说服用户，并不要求解释逻辑上与人类推理一致。

## Experiments / Findings
在人类 ECQA 评测中，GPT-4 解释使错误 second-best answer 的 convincingness 从 2.96 提升到 3.53；Claude 从 3.66 提升到 3.72；GPT-3.5-Turbo 从 3.74 提升到 3.84，三组 paired t-test 均显著 (p < 0.01, dof=499)。GPT-4 解释的 human fluency/correctness 分数也很高，说明“解释写得好”和“解释支持错答案”可以同时发生。

策略分析的高频模式包括 comparative advantage framing、reframing the question、selective evidence 和 confidence manipulation；例如 GPT-4 在 ECQA 中 comparative advantage framing 出现 90/100，selective evidence 79/100，detailed scenario building 63/100。图结构实验显示除 GPT-4/4 Turbo 外，模型很难找到 alternative paths；图复杂度增加后即使 GPT-4 也会在较复杂图上失败，说明 AH 不只是简单演绎能力，也与解释结构和说服策略有关。

## Ablation / Error Analysis
主要消融是 explanation 前后的人类 convincingness、不同 explainer/evaluator、ECQA 与 NLI 数据集，以及图的节点/分支复杂度。自动 proxy 并不总复现人类：例如 NLI 某些条件下 Mixtral 的所有 C_after 分数都为 3.00，显示 evaluator 可能受到 dataset artifact 或评分退化影响。

论文还发现 guardrail 不足以自动防止 AH；让模型避免“直接给答案”而改为辅助人类决策，并不能保证解释不会对错误答案进行 persuasive rationalization。human annotator agreement 较弱，也意味着 AH 的绝对发生率要谨慎解释。

## Limitations
MTurk 样本、ECQA/NLI 和三点量表只是有限社会场景，不能代表所有用户或高风险领域。解释策略 taxonomy 由研究者定义，自动评测 proxy 与人类相关性有限。图实验是受控 toy reasoning，不等同于真实世界知识推理；论文展示了风险存在，却没有给出已经验证的通用防御。

## 两句话总结
作者证明 LLM 可以为错误答案生成流畅、事实表面可信且让人更信服的解释，这种 adversarial helpfulness 是 explanation 评测中独立于普通 faithfulness 的安全风险。高频的选择性证据、问题重构和比较优势框架说明，只检查解释是否自然或有帮助会奖励错误说服，必须加入 answer correctness、反事实和人类依赖风险测试。

## Evidence note
已读取 arXiv 2405.06800 v3 本地 PDF；数字来自 human evaluation Table 1、persuasion strategy Table 2 和 graph structural analysis。
