# 218. NLX-GPT: A Model for Natural Language Explanations in Vision and Vision-Language Tasks

- **Authors:** Fawaz Sammani, Tanmoy Mukherjee, Nikos Deligiannis
- **Venue:** CVPR 2022
- **Paper:** https://doi.org/10.1109/CVPR52688.2022.00814
- **Tags:** Natural Language Explanation, Vision-Language, Faithfulness, Self-Evaluation

## Introduction

传统 NLE 系统先由 task model 预测，再把其输出交给独立 explanation model，解释可能只是事后合理化，且增加参数和延迟。NLX-GPT 试图让同一个紧凑生成模型同时产生 answer 与 explanation，使解释和预测共享视觉条件，并设计不依赖人工标签的 faithfulness evaluation。

## Method / Framework

模型先在大规模 image-caption pairs 上预训练视觉理解，再把 answer prediction 改写成与 explanation 一起的 text generation；不需要额外 region proposal 或独立 task/VL model。作者提出 explain-predict：从 explanation 重新预测 answer 检查一致性；以及 retrieval-based attack：用 explanation 检索/替换输入，测试解释是否携带真正支持预测的信息。

## Baselines / Comparisons

在 VQA-X、ACT-X、e-SNLI-VE 和 VCR 上比较既有 two-model NLE、task-specific explanation model 与不同 visual encoder。指标包括 BLEU-4、METEOR、CIDEr、ROUGE-L、SPICE、BERTScore、任务 accuracy，以及 explain-predict/retrieval attack 的一致性和鲁棒性。

## Experiments / Findings

- NLX-GPT 以更少参数达到更好的综合 NLE 分数，并比当前 SOTA 快约 15 倍；在 VQA-X/ACT-X 等任务上同时预测答案和理由，不再需要独立 task model。
- image-caption pretraining 显著改善小规模 NLE 数据上的视觉理解；CLIP ViT encoder 的结果优于 CLIP ResNet-101、DeiT 和未预训练 ResNet，例如表中 ViT 的 BLEU-4/METEOR/CIDEr/ROUGE 为 28.1/22.6/108.5/50.9。
- explain-predict 与 retrieval attack 暴露“文字流畅但与预测无关”的解释，补足 BLEU/ROUGE 只评价表面相似度的缺陷；但 e-SNLI-VE 上不同指标对人类质量的偏好并不一致。

## Ablation / Error Analysis

消融 image-caption pretraining、e-SNLI-VE concept detection 和 visual encoder。没有预训练时模型容易利用数据偏差生成 generic rationale；没有 concept 信息会损失 entailment 解释。错误还来自 explanation 与 answer 一致但未指出 discriminative visual evidence，正是 retrieval attack 要检测的情况。

## Limitations

单模型联合生成提高效率，但 answer 与 explanation 共享 decoder 仍不能保证后者因果地驱动前者；自评指标也可能被模型自身的偏差欺骗。e-SNLI-VE 某些 NLG 指标略弱于先前方法，且数据集与视觉编码器选择限制外部泛化。

## 两句话总结

NLX-GPT 把答案和自然语言解释放进同一视觉条件生成过程，并用 explain-predict 与 retrieval attack 检验解释是否携带预测依据。它大幅降低参数与速度成本，但语言一致性仍不自动等于 faithful explanation。
