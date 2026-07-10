# 187. Controlling Large Language Models Through Concept Activation Vectors

- **Authors:** Hanyu Zhang, Xiting Wang, Chengao Li, Xiang Ao, Qing He
- **Venue:** AAAI 2025
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/34778
- **Tags:** Concept Activation Vector, Steering, Controllable Generation

## Introduction

prompt 不能稳定控制 toxicity、sentiment、style 和 topic；全量 fine-tuning 成本高，现有 decoding control 又常是粗粒度。GCAV 试图用可解释 concept direction 做轻量 inference-time control。

## Method / Framework

用 concept-positive / negative examples 训练 Concept Activation Vector，推理时在 activation layer 中沿概念方向加减 steering，或移除 toxicity vector。通过控制强度实现连续调节，而不是只做 binary blocking。

## Baselines / Comparisons

比较 prompting、fine-tuning、classifier-guided decoding、representation steering 和不同 layer / strength。任务包括 toxicity reduction、sentiment、linguistic style、topic control，指标为 control accuracy、fluency、perplexity 和 content preservation。

## Experiments / Findings

- GCAV 在多个控制任务上实现细粒度 steer，并在 toxicity / style / topic 指标上达到 SOTA 或竞争表现。
- 不需要大规模 fine-tuning，改变 concept direction 即可调节输出强度。
- layer 选择重要：中间表示通常兼顾 concept separation 与语言流畅性，过早 / 过晚干预会损害内容。

## Ablation / Error Analysis

消融 concept data、layer、steering coefficient、positive / negative examples 和 direction removal。概念相关但非目标的 features 会被一起改变；toxicity 下降可能伴随过度去毒、主题漂移或语义损失。

## Limitations

concept vector 质量依赖标注和 prompt 数据，单一线性方向不能表达复杂概念。白盒访问限制闭源 API，steering 不是 causal guarantee，且恶意用户可能绕过。

## 两句话总结

GCAV 把可控生成转成 activation space 中可读的 concept direction steering，以较低成本控制 toxicity、情感、风格和主题。它比自然语言 prompt 更细粒度，但概念纠缠、数据偏差和白盒依赖仍需解决。
