# 136. Towards Interpretable Mental Health Analysis with Large Language Models

- **Authors:** Kailai Yang, Shaoxiong Ji, Tianlin Zhang et al.
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.370
- **Tags:** Mental Health, Explainability, Emotion, Human Evaluation

## Introduction

心理健康检测属于安全关键任务，错误的 suicide、depression 或 stress 判断可能直接影响人。已有 LLM 工作多只测少量二分类数据，使用简单 prompt，也很少检查解释是否忠实、完整和可靠；cause/factor detection、emotion recognition 和复杂 emotional reasoning 也没有被系统覆盖。

论文的三个问题是：LLM 在 5 类、11 个数据集的心理健康分析任务上有多强？情绪线索、情绪增强 CoT 和专家 few-shot 是否有效？LLM 生成的逐样本解释是否达到人类可接受质量，现有自动指标能否反映这种质量？

## Method / Framework

作者比较 InstructGPT-3 与 ChatGPT（gpt-3.5-turbo），并设计多层 prompt：

- zero-shot 直接要求分类；
- sentiment-enhanced prompt 加入 VADER 情感信息；
- emotion-enhanced prompt 使用 NRC EmoLex 等词典产生更细粒度情绪线索；
- emotion-enhanced CoT 要求先考虑情绪，再输出 Yes/No 和逐步解释；
- emotion-enhanced CoT + few-shot 加入专家书写的示例。

任务覆盖二分类心理状态检测、多分类状态检测、cause/factor detection、emotion recognition in conversations 和 causal emotional reasoning。解释由模型生成，再由 3 位标注者按 fluency、reliability、completeness 评分，形成 163 条人工评估解释数据。作者还比较 BARTScore、ROUGE 等自动指标与人工评分的相关性。

## Baselines / Comparisons

检测任务中与 CNN、GRU、BERT、RoBERTa、XLNet 以及任务特化的 state-of-the-art 方法比较；LLM 内部则比较 zero-shot、sentiment、emotion、CoT 和 few-shot 版本。评价指标是二分类 recall / weighted-F1，多分类与复杂任务主要用 weighted-F1，情绪任务还根据数据集使用 micro-F1、positive / negative F1 和 macro-F1。

## Experiments / Findings

- ChatGPT zero-shot 在二分类任务上明显强于其它通用 LLM 和部分传统 baseline，但仍落后于先进的 task-specific supervised 方法；在复杂多分类、cause detection 和因果情绪任务上差距更明显。
- 表 1 的 ChatGPTZS weighted-F1 大致为 DR 82.41、CLPsych15 56.31、Dreaddit 54.05、SAD 33.30、CAMS 50.29、T-SID 33.85，显示任务难度差异远大于模型名称差异。
- 直接使用 CoT 并不稳定，ChatGPTCoT 在部分数据上比 zero-shot 更差；加入 emotion-enhanced CoT 后，整体表现提高，专家 few-shot 版本在复杂数据上尤其有效，T-SID 和 cause detection 改善明显。
- ChatGPT 的解释在预测正确的样本上接近人类水平：三位标注者对超过 95% 的 ChatGPT 解释给出一致判断；ChatGPTtrue 的总体解释评分超过 2.5，fluency、reliability、completeness 都明显优于 InstructGPT-3。
- 自动指标并不统一：BARTScore 在 fluency / reliability 上更有相关性，ROUGE 对 completeness 或错误语义区分较好，说明单一 overlap 指标不足以衡量解释可靠性。

## Ablation / Error Analysis

作者改变 prompt 中的形容词，发现 ChatGPT 的 weighted-F1 会显著波动，同一数据集上没有一个全局最优修饰词，说明零样本预测对 wording 敏感。情绪增强消融表明粗粒度 sentiment 线索不如细粒度 emotion 线索；但词典标签也会误表示复杂文本中的多重情绪。错误解释常出现“fluently wrong”：文字顺畅，却漏掉关键线索、因果方向错误或依赖外部常识，尤其集中在 ChatGPTfalse 样本。

## Limitations

论文主要使用 zero-shot / few-shot，没有进行 LLM fine-tuning；医疗和心理状态的主观性使数据标签本身不完全可靠。情绪词典可能错配上下文和多轮对话，ChatGPT 版本、prompt wording 和输出随机性会改变结果。作者也警告，生成的心理健康解释不能代替专业诊断，部署需要安全审核和误报 / 漏报控制。

## 两句话总结

论文把心理健康分析从“LLM 给一个标签”扩展为情绪增强推理加人工评估解释，系统覆盖 11 个数据集和 5 类任务。ChatGPT 的解释在正确判断上接近人类，但任务性能仍落后于专门监督模型，且 prompt 敏感、错误推理和医疗风险限制了直接应用。
