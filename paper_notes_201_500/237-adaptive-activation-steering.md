# 237. Adaptive Activation Steering: A Tuning-Free LLM Truthfulness Improvement Method for Diverse Hallucinations Categories

- **Authors:** Tianlong Wang, Xianfeng Jiao, Yifan He, Zhongzhi Chen, Yinghao Zhu, Xu Chu, Junyi Gao, Yasha Wang, Liantao Ma
- **Venue:** WWW 2025
- **Paper:** https://doi.org/10.1145/3696410.3714640
- **Tags:** Activation Steering, Truthfulness, Hallucination, Adaptive Control

## Introduction

LLM 可能“知道”事实却在生成时没有表达出来，不同 hallucination category 也需要不同修复方向；固定一个 steering vector/strength 会过度抑制 helpfulness 或对某类问题无效。ACT 要在不 fine-tune 的情况下，用多种 truthfulness vectors 和输入自适应强度把 activation 推向更真实的区域。

## Method / Framework

ACT 从 TruthfulQA 等数据上学习多种 probe-driven truthfulness directions，并在 inference 中根据输入偏离程度选择/组合方向。Adaptive Steering Intensity Control (ASIC) 决定每个样本的干预强度，配合 diverse steering vectors；它是 add-on，不修改模型参数，并可与 few-shot prompting 组合。

## Baselines / Comparisons

比较 no steering、random steering、CCS、RepE、Mean-Centring、ITI 和 single steering，在 TruthfulQA open-ended generation 与 multiple-choice 上报告 BLEURT、Truth、True*Info、MC1/MC2、cross-entropy/KL。还测试 LLaMA/LLaMA2/Alpaca/Vicuna/LLaMA2-Chat 和 7B/13B/33B/65B scaling。

## Experiments / Findings

- 在 LLaMA-7B few-shot setting，ACT 的 True*Info 为 39.1，高于 baseline 23.0 和 ITI 28.6；full-data setting 相对 baseline True*Info 提升 83%，比最佳比较方法 Mean-Centring 高 34%。
- LLaMA 7B/13B/33B/65B 加 ACT 的 open-ended True*Info 为 46.6/46.0/49.6/50.4，说明方法可扩展但不是单调大幅提升。
- 消融显示 single steering、adaptive intensity、diverse steering 逐步贡献，完整 ACT 在 truthfulness 与 informativeness 间取得较平衡，而强度过大可能损伤流畅度。

## Ablation / Error Analysis

消融 ASIC、diverse vectors、方向数、模型规模与 few-shot prompting。单 vector 对不同 hallucination category 不够，固定 intensity 会过度拒答/损失 helpfulness；方向本身的 probe bias 和 TruthfulQA 覆盖不足会造成错误 steering。

## Limitations

truthfulness metrics 不能完全覆盖开放世界事实、长答案和实时知识；activation steering 需要白盒模型并可能改变 style/content。不同模型的方向可迁移性、对抗 prompt 和 truthfulness-helpfulness trade-off 仍需更广泛评测。

## 两句话总结

ACT 用多条 truthfulness activation directions 加输入自适应强度，在推理时无需训练即可改善多类 hallucination。它在 TruthfulQA 和多种 LLaMA 系列上超过固定/单向 steering，但 probe 偏差和 helpfulness 损失仍需控制。
