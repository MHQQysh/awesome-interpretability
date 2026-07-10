# 116. APPLS: Evaluating Evaluation Metrics for Plain Language Summarization

> 人工精读笔记：EMNLP 2024。重点是把 plain-language summarization 的 metric evaluation 变成受控 perturbation testbed。

## 0. 论文信息

- **作者**：Yue 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.519)
- **数据**：CELLS、PLABA biomedical plain-language summaries
- **方法**：APPLS；评估 informativeness、simplification、coherence、faithfulness

## 1. Introduction：要解决什么问题

PLS 既要忠实传达科学摘要，又要更易读；一个 metric 可能偏爱更短文本，却漏掉 hallucination 或重要信息。现有指标常在真实系统输出上相关性评估，无法知道它是否对某一特定错误敏感。

## 2. Method / Framework：怎么解决

APPLS 对 source/summary 做受控 perturbations：删除或添加句子/定义、实体交换、否定、词汇替换、结构变化等，并尽量只改变一个质量维度。再用 human validation 检查扰动是否确实影响目标 criterion，测试 14 个自动和 prompt-based metrics。

## 3. Baseline / 对比基线

SARI、BLEU/ROUGE、BERTScore、GPT-PPL、LENS、QAEval、SummaC/entailment、LLM prompt scoring；单独 prompt 评价一个 criterion 与同时评价所有 criterion。

## 4. Experiments / Findings：结果如何读

APPLS 显示没有一个 metric 对四个 criterion 都稳定。SARI 对 simplification 有优势，QAEval 对 faithfulness 较好，coherence 需要单独指标；许多 metrics 对 entity swap、局部事实变化不敏感。

Prompt-based scores 能捕捉部分 informativeness/faithfulness perturbation，但追加解释并不总是必要，且可能改变模型的评分偏好。synthetic perturbation 会同时影响多个维度，这是 testbed 设计时必须报告的残余混杂。

## 5. Ablation / 机制解释

删除/添加/否定/实体交换/词汇改写、reference-free vs reference-based、单 criterion vs all criteria、14 个 metric 交叉比较构成主要消融。结果说明 metric meta-evaluation 比单看与人类总体相关更能发现盲点。

## 6. Limitations / 局限与复现注意

APPLS 主要是 biomedical PLS，扰动不可能完全只改变一个维度；专家验证成本高；不同语言和法律/医疗文本可能需要重新设计 perturbation。

## 7. 两句话总结

APPLS 为 plain-language summarization 构建了受控、细粒度的 metric testbed，测试指标是否能识别信息、简化、连贯和忠实性变化。结果表明 SARI/QAEval 等各有专长，没有普适 metric，且合成扰动的多维影响必须纳入解释。
