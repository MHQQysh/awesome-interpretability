# 185. C3oT: Generating Shorter Chain-of-Thought Without Compromising Effectiveness

- **Authors:** Yu Kang, Xianghui Sun, Liangyu Chen, Wei Zou
- **Venue:** AAAI 2025
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/34608
- **Tags:** CoT Compression, Reasoning Cost, Interpretability

## Introduction

CoT 提高 reasoning accuracy，却常比最终答案长很多，增加 latency 和 token cost；直接删短 reasoning 又会丢关键步骤。C3oT 目标是压缩而不是删除，让短 chain 保留有效信息与可解释性。

## Method / Framework

Conditioned Compressed CoT 包含 compressor、conditioned training 和 conditioned inference：先把长 CoT 压成短 CoT，再同时训练长 / 短 chain 的对应关系，推理时用短输出借用长 reasoning 学到的能力。

## Baselines / Comparisons

比较原始长 CoT、直接截断 / summary、self-consistency、不同压缩比例和其它 concise reasoning 方法，在 arithmetic 与 commonsense 四个 dataset 上报告 accuracy、长度、latency 和 rationale quality。

## Experiments / Findings

- C3oT 在明显减少生成 token 的同时保持接近甚至超过长 CoT 的答案 accuracy。
- 相比直接删步骤，conditioned training 能保留关键 intermediate information 和解释性。
- 短 chain 特别适合 latency-sensitive search / recommendation，但复杂问题仍需较长推理。

## Ablation / Error Analysis

消融 compressor、conditioned loss、压缩长度、长短 chain 配对和 inference condition。压缩过度会漏掉变量绑定、算术中间值和反事实步骤；无 condition training 的短 CoT 容易变成无理由结论。

## Limitations

实验任务范围有限，短 rationale 的“解释性”仍可能只是一种摘要。训练需要 teacher long CoT，成本没有消失；复杂多跳和开放域任务的压缩边界尚不清楚。

## 两句话总结

C3oT 用长短 CoT 对齐训练把推理能力压进更短的 rationale，降低 decoding 成本而尽量保留 accuracy。它比截断更稳，但压缩比例、任务复杂度与真实 faithfulness 仍需严格评测。
