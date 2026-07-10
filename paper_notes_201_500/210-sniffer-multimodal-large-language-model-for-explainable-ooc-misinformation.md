# 210. SNIFFER: Multimodal Large Language Model for Explainable Out-of-Context Misinformation Detection

- **Authors:** Peng Qi, Zehong Yan, Wynne Hsu, Mong Li Lee
- **Venue:** CVPR 2024
- **Paper:** https://doi.org/10.1109/CVPR52733.2024.01240
- **Tags:** Multimodal Misinformation, OOC Detection, Retrieval, Explanation

## Introduction

Out-of-context misinformation 不一定篡改图片，而是把真实图片放进错误新闻语境；模型必须检查图片与 caption 的人物、时间、地点和事件是否一致。普通 detector 只给真假，通用 MLLM 又容易把文本和图像表面相似误判为真实，论文目标是同时输出判决、证据和可读解释。

## Method / Framework

SNIFFER 以 InstructBLIP 为 base，经过新闻域预训练和 OOC task instruction tuning。推理时一条路径检查 image-caption 的 internal cross-modal inconsistency，另一条用 image retrieval 找外部网页证据并把 text claim 拆成 fact queries，MLLM 分别回答后再做 composed reasoning；visual entities 和 retrieved evidence 最终共同生成判断与 explanation。

## Baselines / Comparisons

在 OOC misinformation benchmark 上比较 SAFE、EANN、VisualBERT、CLIP、DT-Transformer、CCN、Neu-Sym detector，以及 InstructBLIP、GPT-4V 的随机测试对比。指标为 All/Fake/Real accuracy，另以 human/qualitative evaluation 检查解释是否指出具体不一致元素而非泛泛说“图片不匹配”。

## Experiments / Findings

- SNIFFER 在主表达到 All/Fake/Real 88.4/86.9/91.8，超过 CCN 的 84.7/84.8/84.5；在随机测试子集上为 86.8，超过 GPT-4V 的 75.5。
- 原始 InstructBLIP 只有 47.4% overall、Fake recall 4.6%，OOC tuning 后提升超过 35 个百分点；visual entities 约带来 5 个百分点增益，说明任务适配和显式实体证据都不可缺。
- 外部证据能帮助区分同一人物/事件的错配，输出 explanation 也更具体；但只依赖检索相关性会偏向高 fake recall、损失 real precision，所以必须和内部视觉检查结合。

## Ablation / Error Analysis

逐步加入 news-domain pretraining、OOC tuning、VisEnt 和 Evidence；四者完整组合最好。错误来自检索网页不相关、实体链接错误、历史事件时间混淆、caption 与图像同时含多个人物，以及外部证据本身过时；模型也可能把 retrieval consistency 当成事实真相。

## Limitations

系统依赖网页检索、GPT-4 辅助构造 instruction data 和 InstructBLIP 微调，线上成本及可复现性受外部工具影响。OOC benchmark 主要测试图文语境错配，不能覆盖深度伪造、视频和多语言新闻；解释的 persuasive 不等于事实核验完成。

## 两句话总结

SNIFFER 把 MLLM 的内部图文检查、视觉实体和外部网页证据组合起来，同时判断 OOC misinformation 并生成指出人物/时间/事件冲突的解释。它显著优于传统 detector 与 GPT-4V，但检索噪声、证据时效和解释真实性仍需独立 fact-checker。
