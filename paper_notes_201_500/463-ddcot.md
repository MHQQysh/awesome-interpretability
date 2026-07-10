# 463. DDCoT: Duty-Distinct Chain-of-Thought Prompting for Multimodal Reasoning

- **Authors:** Ge Zheng, Bin Yang, Jiajin Tang, Hong-Yu Zhou, Sibei Yang
- **Venue / year:** NeurIPS 2023
- **Paper:** [arXiv:2310.16436](https://arxiv.org/abs/2310.16436)
- **Tags:** `multimodal-cot` `rationale-faithfulness` `negative-space-prompting` `explainability`

## Introduction
多模态 CoT 既需要识别图像，又需要推理；直接把 caption、问题和 rationale 混在一起容易产生视觉幻觉，人工 rationale 标注又昂贵且缺少 OOD 泛化。DDCoT 试图让“识别”和“推理”各做其事，同时用 negative-space prompting 保持对不确定信息的谨慎。

## Method / Framework
DDCoT 先分解问题并生成 sub-questions，再调用视觉模型获得 recognition 信息，最后由 LLM 负责 reasoning 和 joint integration。细调阶段采用 CLIP ViT-L/14 的 global/local visual features、deep-layer prompting (DLP) 和 rational-compressed visual embedding (RCVE) 对 UnifiedQA 进行多模态对齐；rationale 显式要求模型说明未知/不确定之处，而不是编造视觉事实。

## Baselines / Comparisons
比较 GPT-3/ChatGPT zero-shot、UnifiedQA、MM-CoT、Chameleon，以及 naive rationale、caption/original image、去掉 DLP/RCVE、去掉 uncertainty 和不做 duty distinction 的版本。评估包括 ScienceQA accuracy、OOD generalization、rationale BLEU/ROUGE/similarity 与人工 relevance/correctness/completeness/coherence/explainability。

## Experiments / Findings
细调 ScienceQA 的 DDCoT 为 223M 模型平均 87.34，较实现版 MM-CoT 81.40 高 5.94；IMG/TXT/AVG 为 83.34/91.23/87.34。OOD 三种 held-out domain 上 DDCoT 相对 MM-CoT 提升 15.5、9.6、12.2 个百分点。人工 rationale 评估中 ours 的 relevant/correct/complete/coherent/explainable 为 92.00/86.38/85.71/84.33/83.26%，明显高于 GPT-3 和 MM-CoT。

## Ablation / Error Analysis
caption 只带来有限收益，直接 origin image 反而较弱；naive rationale 的 IMG 75.06，duty-distinct w/o uncertainty 78.19，加入 uncertainty 后 83.34，平均提升到 87.34。interleaved visual prompt 的 authenticity 0.602，而无视觉 naive 为 0.883；duty-distinct/uncertainty 分别恢复到 0.783/0.855。去掉 RCVE 或 DLP 后平均降到 86.06/85.57。

## Limitations
模型仍依赖外部视觉模型、caption/feature 质量和 prompt 设计；人工 explainability 指标主要衡量 rationale 的相关性与可读性，不证明 rationale 是实际内部决策因果。OOD 只覆盖 ScienceQA 的 domain split，不能代表开放世界多模态安全。

## 两句话总结
DDCoT 将多模态 CoT 的识别与推理职责分开，并用负空间/不确定性提示抑制视觉幻觉，在 ScienceQA 上同时提高 accuracy、OOD 泛化和人工解释质量。它的关键贡献不是让 rationale 更长，而是让 rationale 先获得可靠视觉证据，再承担推理。

## Evidence note
已读取动机案例、duty-distinct/DLP/RCVE 结构、主表、泛化、hallucination authenticity、组件 ablation 和解释性人工评估。
