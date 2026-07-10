# 183. Detecting and Mitigating Hallucination in Large Vision Language Models via Fine-Grained AI Feedback

- **Authors:** Wenyi Xiao, Ziwei Huang, Leilei Gan et al.
- **Venue:** AAAI 2025
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/34744
- **Tags:** LVLM, Hallucination, AI Feedback, DPO

## Introduction

LVLM hallucination 既可能是句子与图像冲突，也可能是图像没有支持的外部补充；粗粒度 rejection 或昂贵人工标注难以训练系统化防御。论文提出细粒度 sentence-level AI feedback，并区分 hallucination severity。

## Method / Framework

用 proprietary model 生成小规模 sentence-level hallucination annotations，训练 detector；再通过 detect-then-rewrite 构造 preference pairs。HSA-DPO 将严重度加入 preference loss，优先修复 critical hallucination，而不是把所有不完美样本同等处理。

## Baselines / Comparisons

与 Silkie、GPT-4V、RLHF-V 和其它 LVLM hallucination detection / mitigation 方法在 MHaluBench、Object HalBench、AMBER 上比较，指标包括 detection、object hallucination、severity 和生成质量。

## Experiments / Findings

- HSA-DPO 在 MHaluBench detection 上超过 GPT-4V / Gemini，并在 Object HalBench、AMBER 的 mitigation 指标上超过对照。
- sentence-level feedback 比整段好坏标签提供更精确的 rewrite signal。
- severity-aware preference 让训练更重视严重错误，同时尽量保留无害的表达差异。

## Ablation / Error Analysis

消融 detector、rewrite、severity weight、feedback model 和 preference construction。错误集中在细粒度视觉关系、计数、遮挡和图像中未明确的常识；AI feedback 错误会被 preference training 放大。

## Limitations

proprietary teacher 带来成本、可复现性和偏差；检测器与生成器可能共享 evaluator shortcut。severity rubric 和 benchmark 主要是视觉问答，不能代表所有多模态对话和真实安全风险。

## 两句话总结

论文把 LVLM hallucination 拆成句子级错误和严重度，再用 HSA-DPO 优先修复最危险的错误。结果强于多个 baseline，但 teacher feedback、视觉细节和 benchmark 外部效度仍是风险。
