# 418. Measuring Faithfulness in Chain-of-Thought Reasoning

- **Authors:** Tamera Lanham et al.
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2307.13702
- **Tags:** `CoT` `faithfulness` `rationale` `intervention` `reasoning`

## Introduction
CoT 经常提高 LLM answer accuracy，因此被当作 reasoning explanation；但只有当模型真正根据这段 rationale 得出答案时，文字才是 faithful。本文用对 CoT 的干预观察答案变化，区分 post-hoc rationale、隐藏在 test-time compute 中的收益、编码在措辞中的信息和真正可依赖的推理。

## Method / Framework
作者对 CoT 做四类测试：**early answering** 截断 rationale 后回答；**adding mistakes** 在原 rationale 插入错误并让模型继续；**paraphrasing** 重写 rationale；**filler tokens** 用无信息句点替换 rationale。若答案随关键 reasoning 改变，说明模型至少使用了它；若不变，可能是 post-hoc 或未依赖 rationale。

实验使用 175B helpful-dialog model 和 8 个 multiple-choice tasks，包括 ARC、AQuA、HellaSwag、LogiQA、MMLU、TruthfulQA 等，并比较模型规模和任务难度。

## Baselines / Comparisons
比较 standard CoT、direct answer、filler、paraphrased CoT、错误 CoT 和不同 model sizes。指标是 CoT accuracy gain、answer change after intervention、mistake propagation 和 faithfulness pattern，而非只看 CoT 的最终准确率。

## Experiments / Findings
七个任务中 CoT 提高准确率，但 faithfulness 跨任务差异很大；有些模型强依赖 rationale，有些几乎忽略它。filler token 不能完全复现所有 CoT gain，说明收益不只是额外 test-time compute；paraphrase 通常保持性能，表明具体措辞不一定是隐藏 code。

随着模型变大，论文在多数任务上观察到 reasoning faithfulness 反而下降；小模型有时更 faithful。总体结论不是 CoT 永远不忠实，而是可以通过模型大小、任务和 prompting 找到较忠实条件，且必须先测量再使用。

## Ablation / Error Analysis
CoT 截断位置、插错位置、paraphrase、filler 长度、模型规模和 task difficulty 是核心消融。一个测试不能证明 faithfulness：early answering 可能受提示变化影响，adding mistakes 可能引入不自然文本，paraphrase 可能改变语义；作者采用 defense-in-depth 组合证据。

## Limitations
行为干预只能排除部分不忠实假设，不能直接观察模型真实内部 reasoning。实验主要是 multiple-choice、单一 175B 模型系列和旧版 decoder LM；开放式生成、agentic reasoning 和中间层因果机制未覆盖。

## 两句话总结
本文通过截断、插错、改写和填充 CoT，测试答案是否真正依赖模型声称的推理，发现 faithfulness 跨任务/模型差异很大且更大模型未必更忠实。它把 rationale 解释从“看起来合理”推进到可干预行为测试，但仍不能单独证明完整内部因果链。

## Evidence note
已读取 arXiv 2307.13702 v1 本地 PDF 的四类 intervention、8 个任务、模型规模比较和 defense-in-depth limitations。
