# 398. Large Language Model Safety: A Holistic Survey

- **Authors:** Dan Shi, Tianhao Shen, Yufei Huang, Zhigen Li, Yongqi Leng, Renren Jin, Chuang Liu, Xinwei Wu, Zishan Guo, Linhao Yu, Ling Shi, Bojian Jiang, Deyi Xiong
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2412.17686
- **Tags:** `survey` `LLM-safety` `interpretability` `jailbreak` `governance`

## Introduction
LLM 被部署到高风险社会系统后，安全问题不只是 hallucination，而包括价值不对齐、社会偏见、隐私、毒性、jailbreak、滥用、weaponization、misinformation、deepfake 和 autonomous-agent risk。单一 safety benchmark 无法覆盖从训练数据到部署政策的整个生命周期。

## Method / Framework
survey 以四个基础类别建立主 taxonomy：value misalignment、robustness to adversarial attacks、misuse、autonomous AI risks；再扩展 agent safety、interpretability for safety、技术 roadmap 和 governance。

解释性章节按用途区分：分析 concept formation/storage、in-context learning、generalization/emergence；用于 safety auditing；用于 hallucination、privacy、toxicity、bias alignment；同时讨论 interpretability 的 dual-use、adversarial attack、overtrust 和 accelerating uncontrollable risks。每个安全类别都整理 threat definition、mitigation、evaluation 和 future directions。

## Baselines / Comparisons
这是 survey，不重跑一个 safety model。比较范围包括 supervised alignment/RLHF、refusal/guardrail、red teaming、black-box/white-box jailbreak、external safeguard/internal protection、data filtering、unlearning、monitoring 和 policy/governance。它强调 utility-safety tradeoff：拒答率提高不一定代表真正安全，可能降低有益回答；解释性提升也可能暴露攻击面。

## Experiments / Findings
综述没有统一实验数字，主要 finding 是 LLM safety 必须是多层系统：训练数据与目标、模型内部能力、推理时防御、agent/tool environment、部署监控和社会治理相互作用。Jailbreak 与 autonomous risks 说明静态 benchmark 容易被适应，持续 red teaming 和多场景 evaluation 更重要。

解释性被定位为 safety enabler：机制分析可以定位概念、检测后门、分析 hallucination/偏见并辅助 alignment；但一个错误的解释会制造过度信任，开放内部状态也可能帮助攻击者定位薄弱点。因此 interpretability 既是防御工具也是 dual-use capability。

## Ablation / Error Analysis
survey 建议按生命周期消融安全系统：移除 data filtering、external guardrail、internal representation intervention、red-team loop 或 monitoring，观察风险是否转移。评价要覆盖 paraphrase、multi-turn、跨语言、多模态、tool use 和 agent planning，而不是只测一个拒答 benchmark。

错误模式包括 over-refusal、training-data bias、jailbreak transfer、可解释性方法的 false assurance、评测器/benchmark contamination 和模型在部署环境中的 situational awareness。治理层也可能与技术评估目标不一致。

## Limitations
作为大范围 survey，分类和引用数量多但无法统一验证每个 mitigation 的实际效果；安全定义和风险容忍度依赖社会/应用场景。不同 benchmark、模型版本和攻击者预算不一致，不能直接比较排行榜。解释性章节提出的机制方向仍需要可复现的 causal safety audits。

## 两句话总结
这篇 survey 将 LLM safety 组织为价值不对齐、对抗鲁棒、滥用、自主风险，并把解释性、agent、技术路线和治理纳入同一生命周期框架。它的关键警告是 interpretability 同时可能发现风险、支持 alignment、造成 overtrust 或暴露攻击面，必须和 red teaming、监控及治理一起评估。

## Evidence note
已读取 arXiv 2412.17686 本地 PDF 的摘要、目录、四类 safety taxonomy、interpretability 章节与 roadmap/governance 讨论；本文没有虚构统一 benchmark 数字。
