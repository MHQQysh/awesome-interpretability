# 204. Devils in Middle Layers of Large Vision-Language Models: Interpreting, Detecting and Mitigating Object Hallucinations via Attention Lens

- **Authors:** Zhangqi Jiang, Junkai Chen, Beier Zhu, Tingjin Luo, Yankun Shen, Xu Yang
- **Venue:** CVPR 2025
- **Paper:** https://doi.org/10.1109/CVPR52734.2025.02328
- **Tags:** LVLM, Object Hallucination, Attention Lens, Inference Intervention

## Introduction

LVLM 的对象幻觉通常被归因于语言先验，但视觉信息在模型内部如何被传递、在哪一层开始偏离并不清楚。论文用 attention lens 逐层观察 image tokens、object tokens 和文本语义的交互，试图回答视觉信息是如何被 enriched/refined 的，以及这些内部信号能否同时做幻觉检测和免训练修复。

## Method / Framework

逐层分析发现 middle layers 可分为 visual information enrichment 和 semantic refinement：前者把视觉证据传播到 object tokens，后者用文本语义解释视觉信息。作者观察到真实对象 token 在 enrichment 阶段持续获得更高 attention，而幻觉 token 常由与不一致对象交互的 attention heads 产生；于是用跨 head 的绝对 attention 平均值增强一致 image-token 互动，在推理时调整中间层 image attention。

## Baselines / Comparisons

在 LLaVA-1.5-7B/13B、Shikra-7B、MiniGPT-4-7B 上，对比 greedy、beam、OPERA、VCD 和 PAI。使用 COCO 2014 val 的 500 张图，以 CHAIR sentence/instance hallucination、F1、AUROC、accuracy、recall 衡量，并在不同 layer range 和 intervention strategy 上消融。

## Experiments / Findings

- LLaVA-1.5-7B 的 hallucination detection 在 middle layer range 5--18 上 accuracy/AUROC/recall 为 84.46/90.19/72.34，优于只看浅层或后层；这为中间层“视觉富集”提供了可操作信号。
- 在三种 LVLM 上，方法平均 CHAIR 更低且 F1 更高；LLaVA-1.5-7B 的 CS/CI/F1 为 25.0/6.7/76.1，相比 greedy 的 53.0/15.6/76.7，显著减少对象幻觉且保持描述能力。
- layer 5--18 明显优于 0--4、19--26、27--31；把多个 heads 的一致信息聚合比 PAI 的单独干预更有效，说明“哪一层、哪些 head 在处理视觉证据”比盲目抑制 attention 更重要。

## Ablation / Error Analysis

消融 layer range、head aggregation、intervention coefficient 和与 PAI 的替换；19--26 或 27--31 的干预甚至可能放大幻觉。系数过小修复不足，过大损害描述丰富度，约 0.5 在模型上较平衡。错误仍来自视觉对象不清、多个对象关系冲突、attention pattern 的模型依赖和 CHAIR 只覆盖 object hallucination。

## Limitations

只在若干开源 LVLM 和 COCO caption 场景验证，不能直接推断对话、OCR、关系或长视频幻觉。attention lens 与 attention intervention 是机制线索而非严格因果证明；每个模型的 middle-layer range 和最佳系数都可能不同，且保存 attention map 会增加显存开销。

## 两句话总结

论文把 LVLM 幻觉定位到中间层的视觉信息富集/语义精炼过程，并用跨 head 的 image attention 一致性做检测与免训练修复。它在多个 LVLM 上降低 CHAIR 幻觉，但层范围、模型依赖和 attention 非因果性仍限制了通用结论。
