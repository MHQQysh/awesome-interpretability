# 180. Distilling Text Style Transfer With Self-Explanation From LLMs

- **Authors:** Chiyu Zhang, Honglong Cai, Yuezhang Li, Yuexin Wu, Le Hou, Muhammad Abdul-Mageed
- **Venue:** NAACL SRW 2024
- **Paper:** https://aclanthology.org/2024.naacl-srw.21
- **Tags:** Style Transfer, Self-Explanation, CoT, Distillation

## Introduction

Text Style Transfer 需要改变 style、保持 content 和 fluency，但平行数据稀缺；大型 LLM 有很强 ICL 能力，却太贵、太慢。CoTeX 试图把 LLM 的改写和 reasoning distilled 到 compact student，并让 transfer 过程可解释。

## Method / Framework

CoTeX 用 LLM + CoT prompt 生成 pseudo-parallel source-target pairs 和 self-explanations，说明哪些表达被修改、哪些内容应保持。再用这些数据训练小模型，兼容 parallel 与 non-parallel data；推理时输出 target style 结果与解释。

## Baselines / Comparisons

比较 unsupervised style transfer、supervised fine-tuning、knowledge distillation、ICL LLM、instruction-tuned LLM 和没有 self-explanation 的 student。四个 TST datasets 上评价 style accuracy、content preservation、fluency、overall quality 和 explanation transparency。

## Experiments / Findings

- CoTeX 在四个数据集、尤其低资源 setting 中超过传统 supervised 和 distillation baseline。
- self-explanation 帮助 student 区分真正的 style edits 与不应改变的 content，减少过度重写。
- LLM 生成的 pseudo-parallel data 能替代部分人工 parallel data，但数据质量和 style prompt 对收益影响很大。
- CoT 提供的编辑理由能让 transfer decision 更容易被人检查，而不是只输出黑盒句子。

## Ablation / Error Analysis

消融 pseudo data、CoT、self-explanation、parallel data、style classifier 和 student size。没有 explanation 时内容保持变差；style 定义模糊、长文本和多属性 style 会产生漏改或过改。

## Limitations

teacher LLM 的 style judgment 和 rationale 可能错误，synthetic data 可能放大偏见。实验语言 / style 覆盖有限，解释质量主要是自动或案例分析；小模型不一定能复现大模型的复杂语用推理。

## 两句话总结

CoTeX 用 LLM 的自解释 CoT 生成伪平行数据，把 style transfer 能力蒸馏到更小、更便宜的模型。实验说明 rationale 有助于同时保持内容和改变风格，但 teacher 偏差、低资源数据质量和跨风格泛化仍需验证。
