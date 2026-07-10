# 396. Crafting Interpretable Embeddings by Asking LLMs Questions

- **Authors:** Vinamra Benara, Chandan Deep Singh, John X. Morris, Richard Antonello, Ion Stoica, Alexander G. Huth, Jianfeng Gao
- **Venue / year:** NeurIPS, 2024
- **Paper:** https://arxiv.org/abs/2405.16714
- **Tags:** `LLM` `representation` `interpretable-embedding` `QA-Emb` `neuroscience`

## Introduction
普通 text embedding 很强，却无法告诉用户每个维度代表什么；在语言神经科学中，研究者要用文本表征预测 fMRI voxel，如果 embedding 黑箱，就很难把模型特征和脑区选择性联系起来。QA-Emb 的问题是能否把每个 embedding feature 变成一个人类可读、可检查的 yes/no question。

## Method / Framework
给定文本 x，QA-Emb 用 GPT-4/LLaMA 等自回归 LLM 依次回答一组自然语言问题，例如“Does the sentence mention time?”，把 yes/no 转成二值向量。问题集合 Q 不是通过学习大矩阵得到，而是由 LLM 根据下游任务、领域知识和高误差样本生成，再用 ridge regression/Elastic Net 选择有用问题。

在 fMRI 设置中，文本 embedding 与 3 位受试者听故事的 voxel response 对齐；每个问题的 regression weight 可画在 cortex 上。作者还把 674 个问题蒸馏到一个带多个 classification heads 的 RoBERTa，降低逐问题调用 LLM 的成本，并测试信息检索和文本聚类。

## Baselines / Comparisons
主要 baseline 是 neuroscience 中的可解释 Eng1000 embedding，以及黑箱 BERT、LLaMA 系列 text embeddings。fMRI 任务用 test correlation 评估 voxel response prediction；retrieval 比较 BM-25、QA-Emb 和组合模型；解释性则比较 feature 数量和问题文本是否能被人类检查。

## Experiments / Findings
在 fMRI 预测中，QA-Emb 平均 test correlation 约 0.116，比 Eng1000 提升 26%，略高于 BERT；最佳 LLaMA 黑箱 embedding 仍可能更强，但不可直接解释。更重要的是，QA-Emb 只用 29 个选定问题就达到约 0.122 的平均 correlation，超过 Eng1000 的约 0.118（其特征数约 985）。

问题权重在 cortex 上呈现跨受试者稳定的语言选择性，例如 grammatical complexity、physical action 等问题与已知语言相关脑区对应。retrieval 中 BM-25 + QA-Emb 达到约 0.80/0.71/0.84 的三项指标，略优于可解释 baseline；文本聚类中让 GPT-4 从问题集合选任务相关子集后表现提升。

## Ablation / Error Analysis
问题数量是关键消融：从大量候选逐步选择到 29 个仍能保持/提高 fMRI 预测，说明自然语言问题可以做紧凑 bottleneck。蒸馏的 binary/probabilistic QA-Emb 保留大部分性能，说明单次多头模型能替代大量 LLM calls，但会引入 teacher-answer error。

作者在 54 个 binary classification datasets 上测 question-answering faithfulness，发现现代 LLM 平均准确率较高但不同问题差异很大，有些问题接近 chance；如果 LLM 不能忠实回答问题，embedding feature 的语义标签就不可靠。多问题聚合能缓解个别错误，却不能保证每个维度都是真实属性。

## Limitations
原始 QA-Emb 每个维度需要一次 LLM call，计算成本高，强依赖 LLM 能否可靠回答自然语言问题。问题集合的质量取决于 prompt 和领域知识，可能遗漏重要因素或引入研究者偏见。fMRI correlation 只能说明预测和神经响应相关，不能证明问题 feature 是脑编码机制。

## 两句话总结
QA-Emb 用 LLM 对一组人类可读 yes/no 问题作答，把黑箱 text embedding 变成可检查的二值语义特征，并在 fMRI 预测上以更少问题超过 Eng1000、接近/略超 BERT。它把解释性写入表示定义，但问题回答错误、调用成本和相关性不等于因果机制仍是主要限制。

## Evidence note
已读取 arXiv 2405.16714 本地 PDF 的方法、fMRI/IR/聚类实验、29-question 压缩结果、蒸馏和 faithfulness 分析。
