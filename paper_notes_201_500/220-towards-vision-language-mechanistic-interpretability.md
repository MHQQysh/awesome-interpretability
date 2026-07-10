# 220. Towards Vision-Language Mechanistic Interpretability: A Causal Tracing Tool for BLIP

- **Authors:** Vedant Palit, Rohan Pandey, Aryaman Arora, Paul Pu Liang
- **Venue:** ICCV 2023 Workshop
- **Paper:** https://doi.org/10.1109/ICCVW60793.2023.00307
- **Tags:** Mechanistic Interpretability, Causal Tracing, BLIP, VQA

## Introduction

语言模型的 causal tracing 能通过 corruption/patching 定位事实和行为的因果中介，但 vision-language model 的视觉 embedding、question encoder、cross-modal fusion 和 answer decoder 让原工具不能直接使用。论文把这种方法适配到 BLIP，回答图像条件问答的答案到底由哪些层和 token state 恢复。

## Method / Framework

先把 clean image embedding 加噪得到 corrupted run，使 BLIP 输出错误答案；再把 clean run 的单个中间 state patch 回 corrupted run，测答案概率恢复幅度。对 image patch embedding、question encoder 和 answer decoder 的 layer/token 做系统扫描，恢复效果大的位置被视为 causally relevant，形成 BLIP causal tracing heatmap/tool。

## Baselines / Comparisons

方法沿用 Meng 等人的 unimodal causal tracing，与 attention/probing 类 VLM interpretability 工作在问题设定上区分；实验集中于 COCO-QA 的 color identification、location identification、object counting。BLIP 原始任务性能约为 color 80.23%、location 26.30%、counting 3.27%，作者主要以 clean/corrupted answer probability difference 为 causal 指标。

## Experiments / Findings

- 在 200 个样本、10 次重复下扫描噪声系数，选择噪声因子 5，在破坏足够输出的同时避免 patching 变成 trivial restoration；答案恢复 heatmap 显示后层 representation 对所有 answer tokens 更具因果相关性。
- 不同任务的中间位置分布不同，但视觉信息经过 question encoder/cross-modal layers 后在后层被整合进答案；这比单看 attention 更接近“改动该状态会不会改变答案”的机制证据。
- 作者开源 BLIP tracing tool，给后续研究视觉 token、语言 token 和跨模态 circuit 提供可复用接口。

## Ablation / Error Analysis

消融 noise factor、patch layer、question/answer token、BLIP component 和不同任务；噪声过小让 clean patching 近似无效，过大则无法恢复。任务性能低的 counting 会产生弱/不稳定 heatmap，且 patch 单个 state 可能破坏 residual/cross-attention 的自然依赖。

## Limitations

实验只覆盖 BLIP 和 COCO-QA 子集，不能外推到现代 LVLM 或开放式生成；causal tracing 结果依赖噪声模型、patching location 与恢复指标。后层高相关可能是信息汇聚的结果而不是单一机制，工具仍需多模型、反事实和 circuit-level 验证。

## 两句话总结

论文把 corruption-and-patching causal tracing 适配到 BLIP，用答案恢复幅度定位图像、问题和后层表示对 VQA 的因果贡献。它提供了从 attention 可视化走向干预证据的工具，但噪声选择、模型规模和复杂生成任务的可迁移性仍有限。
