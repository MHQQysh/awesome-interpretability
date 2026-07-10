# 219. VL-InterpreT: An Interactive Visualization Tool for Interpreting Vision-Language Transformers

- **Authors:** Estelle Aflalo, Meng Du, Shao-Yen Tseng, Yongfei Liu, Chenfei Wu, Nan Duan, Vasudev Lal
- **Venue:** CVPR 2022
- **Paper:** https://doi.org/10.1109/CVPR52688.2022.02072
- **Tags:** Interpretability Tool, Vision-Language Transformer, Attention, Visualization

## Introduction

NLP Transformer 已有注意力和 hidden-state 可视化，但 vision-language Transformer 的跨模态交互、层间表示和失败原因仍难以观察。VL-InterpreT 的目标不是提出新模型，而是提供 task-agnostic、可交互的工具，让研究者能同时追踪视觉/语言 attention 和 token representation。

## Method / Framework

工具记录所有层和 attention heads 的 intra-modal/cross-modal 统计，绘制 vision-to-language、language-to-vision 与模态内部的 heatmap，并把 image/text token hidden representations 随层变化画成轨迹。用户可以输入新图文、逐层浏览、对比 attention/表示和输出，从而定位模型在哪一步形成或丢失跨模态概念。

## Baselines / Comparisons

论文以 KD-VLP end-to-end multimodal Transformer 为 case study，在 Visual Commonsense Reasoning (VCR) 与 WebQA 上展示工具功能；不是传统 accuracy benchmark，而是与已有 NLP interpretability、attention visualization 和 probing 工作在功能覆盖上比较。案例关注 learned cross-modal concepts、attention heads、hidden-state clustering 与失败样例。

## Experiments / Findings

- 工具能显示不同层的模态交互：某些 heads 关注图像区域与问题词的对应关系，另一些 heads 更偏语言上下文，帮助研究者区分视觉证据与语言捷径。
- VCR/WebQA case studies 展示了模型如何形成跨模态概念，也能发现错误预测时 attention 没有落到关键区域或 hidden representation 未形成正确语义。
- 后续版本支持实时运行模型和交互修改输入，使工具从静态论文图扩展到可复现实验和假设检验环境。

## Ablation / Error Analysis

工具本身通过选择不同 layer/head、统计项和输入扰动进行对照；可观察到 attention heatmap 依赖 head 聚合方式，hidden-state 可视化依赖降维/投影选择。它能暴露模型错误，但不能仅凭 attention 判断因果，需配合 token masking、representation intervention 或输出变化。

## Limitations

可视化是分析接口而非自动机制解释，用户仍需提出假设并验证；attention 与 hidden representation 的可读性也受模型结构和降维方法影响。case study 主要围绕 KD-VLP/VCR/WebQA，现代 LVLM、生成长链和大规模统计尚未覆盖。

## 两句话总结

VL-InterpreT 把视觉/语言 attention、跨模态 heatmap 和 token hidden-state 轨迹整合成可交互的 Transformer 解剖工具。它能帮助发现跨模态概念和失败模式，但可视化线索必须通过干预验证，不能把 attention 直接当作因果解释。
