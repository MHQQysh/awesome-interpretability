# 466. Integrating Automated Knowledge Extraction with Large Language Models for Explainable Medical Decision-Making

- **Authors:** Haodi Zhang, Jiahong Li, Yichi Wang, Yuanfeng Song
- **Venue / year:** IEEE BIBM 2023
- **Paper:** [Paper PDF](https://hdzhangust.github.io/static/files/papers/2023%20BIBM%20Integrating%20Automated%20Knowledge%20Extraction.pdf)
- **Tags:** `medical-diagnosis` `LLM-knowledge-extraction` `Markov-logic-network` `symbolic-explanation`

## Introduction
深度模型能提升疾病诊断，却难以解释“症状如何导致诊断”；仅让 ChatGPT 直接给答案也无法提供可审计的规则。论文将 LLM 的知识提取/形式化能力与 Markov Logic Network (MLN) 结合，生成带权一阶逻辑规则，并由 MLN 做最终可解释推理。

## Method / Framework
ChatGPT-3.5 总结疾病知识，embedding 检索相关医学文本，GPT-4 将知识转成 FOL，迭代优化规则；Alchemy 学习 MLN 权重，MCMC 推理疾病边缘概率。Diaformer 先模拟问诊、提出症状问题，框架把 true-positive symptom sequence 送入 MLN，最终给出最高后验诊断并展示触发的 FOL 规则。

## Baselines / Comparisons
数据为 Muzhi、DXY 和 Synthetic；比较 SVM-exp、Flat-DQN、HRL、KR-DS、GAMP、PPO、Diaformer，以及有条件地给出真实症状序列的 ChatGPT/ChatGPT+self-consistency。指标是 classification accuracy，并加上去除 knowledge formalization、summary、iterative optimization 的 ablation。

## Experiments / Findings
三数据集 accuracy 为 SVM 67.3/64.0/34.1，Diaformer 74.2/83.9/73.3，ours 76.4/84.9/72.9；平均 78.0，高于 Diaformer 77.1。ChatGPT+self-consistency 在 Muzhi/DXY 平均 66.6，ours 为 80.7，说明结构化知识和 MLN 比直接生成诊断更可控；Synthetic 上略低于 Diaformer，显示规模增大时有解释性代价。

## Ablation / Error Analysis
普通 MLN 只有 28.4/30.3/16.4；去除 summary 与 iterative optimization 为 48.6/54.8/46.4，去 IO 为 50.2/60.0/61.8，去 summary 为 73.0/78.8/68.2，完整模型为 76.4/84.9/72.9。summary 负责压缩上下文并减少 LLM 形式化错误，IO 负责迭代修正规则；规则 case study 能把 ankle pain、fever 等症状连到 dengue fever 的显式逻辑链。

## Limitations
LLM 抽取的知识和 FOL 仍可能错误，MLN 只能在提供的规则中解释；Synthetic 上的下降显示规则数量与复杂度会伤害性能。实验允许 ChatGPT 直接看到 true-positive 症状序列，实际问诊噪声和临床安全尚需验证。

## 两句话总结
论文把 LLM 变成知识提取器，把最终诊断交给可读的 MLN/FOL 推理，从而将“生成式直觉”转成可检查规则。它在两个真实数据集上超过传统 baseline，但规则质量高度依赖 summary、迭代修正和外部医学知识。

## Evidence note
已读取本地 BIBM PDF 的数据、LLM/MLN pipeline、accuracy 表、ChatGPT 对照、ablation 和 FOL case study。
