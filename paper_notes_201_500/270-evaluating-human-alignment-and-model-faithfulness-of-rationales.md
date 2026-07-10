# 270. Evaluating Human Alignment and Model Faithfulness of LLM Rationale

- **Authors:** Mohsen Fayyaz, Fan Yin, Jiao Sun, Nanyun Peng
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2407.00219
- **Tags:** Rationale Evaluation, Human Alignment, Faithfulness, Attribution, Fine-Tuning

## Introduction

LLM 可以用 prompt 生成 rationale，也可以用 attention/gradient attribution 选出影响预测的 input tokens，但两者是否真的贴近人类证据、是否反映模型自己的决策并不清楚。论文同时评价 human alignment 与 model faithfulness，并特别问 fine-tuning 提高分类准确率后，rationale 是否也变得更可靠。

## Method / Framework

实验覆盖 e-SNLI、FEVER、MedicalBios 三个带 rationale annotation 的文本分类数据集，以及 Llama2、Llama3、Mistral、GPT-3.5-Turbo、GPT-4-Turbo。prompting-based 方法包括 guided/unbound self-explanation；attribution-based 方法包括 raw attention、Saliency、Input×Gradient 等。Human alignment 用模型选词与人工 rationale 的 token F1；faithfulness 通过 mask 掉 rationale 后观察预测是否 flip，另外跟踪 fine-tuning 10 epochs 的 accuracy、alignment 和 flip rate。

## Baselines / Comparisons

基线横跨 prompt rationale、attention weights、gradient saliency、Input×Gradient 和 random Top-Var selection；模型横跨开源/闭源与 pre-trained/fine-tuned 条件。实验同时报告预测准确率，避免在模型几乎只输出一个 label 时把低 flip rate 错当成“解释不重要”或“模型不忠实”。

## Experiments / Findings

- attribution-based rationales 在大多数模型/数据集上比 prompting-based 更贴近 human rationale，尤其是 fine-tuning 后；Input×Gradient 和 Saliency 通常优于 raw attention。
- fine-tuning 提高分类准确率时，attribution alignment 更一致地上升；prompting explanation 不显示同样稳定的趋势，说明模型更会做题不等于会自然语言解释自己的证据。
- 预训练模型若 accuracy 很低或存在 label collapse，mask token 后预测仍不变，faithfulness flip rate 会被压到接近 0；因此论文主张先检查 task performance，再解释 faithfulness。经过 fine-tuning 后，attribution 方法在较可靠的评估区间内普遍优于 prompting。

## Ablation / Error Analysis

随机 Top-Var、选择 token 数量、是否 fine-tune 和 rationale 方法是主要消融。prompt rationale 的 token 选择受提示措辞和模型阈值影响，attribution 也可能只反映局部梯度而非人类可读因果理由；masking 一整个 rationale 可能产生分布外输入。人类 rationale 的 flip rate 也不是模型内部因果的黄金真值，需结合 accuracy 和不同 masking policy。

## Limitations

研究主要是 extractive rationale，不能覆盖 free-form explanation；token F1 受人工标注粒度影响，faithfulness flip 受模型分类能力、label bias 和 prompt template 影响。GPT-4 的闭源 attribution 不完全可比，且 fine-tuning 10 epochs 的趋势不代表所有训练阶段或真实部署。

## 两句话总结

这篇工作系统比较了 prompt 自解释与 attention/gradient attribution，发现 attribution 尤其 Input×Gradient 更常同时贴近人类证据和模型决策。它还揭示低准确率/label collapse 会让传统 faithfulness flip evaluation 失效，因此解释评价必须与模型任务性能和训练状态一起报告。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、rationale methods、Tables 1-5、fine-tuning/faithfulness analysis 与 Conclusion；结论依据正文的趋势性报告，未补写图中无法稳定读取的单元格数值。

