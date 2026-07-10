# 233. Exploring Mechanistic Interpretability in Large Language Models: Challenges, Approaches, and Insights

- **Authors:** Paper metadata lists an ICDSAAI 2025 survey contribution
- **Venue:** ICDSAAI 2025
- **Paper:** https://doi.org/10.1109/icdsaai65575.2025.11011640
- **Tags:** Mechanistic Interpretability, Survey, Circuits, Activation Steering

## Introduction

LLM 的行为分数和自然语言 rationale 不能直接告诉我们模型如何计算；mechanistic interpretability 试图从 representation、attention、MLP、circuits 和 intervention 反向工程算法。该文的目标是整理研究问题、主流方法和已经获得的机制洞见，并指出从小模型结论外推到大模型的风险。

## Method / Framework

综述按 activation/feature visualization、probing/representation analysis、attention/attribution、circuit discovery、causal tracing/patching、SAE 和 activation steering 组织方法，串联“定位 feature -> 形成 circuit 假设 -> 干预验证 -> 行为改进”的研究流程。它同时讨论 interpretability 与 safety/alignment、faithfulness 和 scalability 的关系。

## Baselines / Comparisons

作为 survey，比较的不是统一分数，而是各方法的观察对象、因果强度、可解释粒度、计算成本和对模型访问的要求。black-box rationale/attention 作为较弱的行为解释，对照于 white-box neuron、head、feature、circuit 和 causal intervention。

## Experiments / Findings

- 现有工作已在 induction、知识回忆、copying、sentiment/toxicity 和 steering 等局部行为中发现可复用结构，但结果常依赖 task prompt、模型规模和人工选择。
- SAE 缓解 polysemanticity，causal patching/steering 能将相关性推进到干预证据；然而 probe 可解码不等于模型使用，attention 高不等于因果贡献。
- 综述强调自动化解释、跨模型 circuit 对齐、统一 faithfulness metric 和将机制发现用于 alignment 是主要未解方向。

## Ablation / Error Analysis

综述本身没有新统一 ablation；它的批判性分析指出 attribution/attention 容易受 baseline、聚合和 tokenization 影响，SAE feature label 受解释器 bias 影响，patching 受 corruption 位置影响。任何单一方法都应与 activation intervention 和行为反事实结合。

## Limitations

 survey 结论依赖已有论文质量，许多机制结果来自小模型/局部任务，尚缺跨架构 ground truth。对 emergent abilities、开放式生成和 pluralistic alignment 的机制解释仍处于早期。

## 两句话总结

本文把 LLM mechanistic interpretability 组织成从 feature/circuit 定位到 causal intervention 和 steering 的方法谱系，并明确区分行为相关与机制因果。它适合建立研究地图，但不能替代对具体模型、任务和原始实验的逐篇核验。
