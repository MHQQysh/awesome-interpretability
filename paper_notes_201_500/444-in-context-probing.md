# 444. In-Context Probing: Toward Building Robust Classifiers via Probing Large Language Models

- **Authors:** Afra Amini, Massimiliano Ciaramita
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2305.14171
- **Tags:** `in-context-learning` `probing` `robustness` `low-resource-classification`

## Introduction
ICL 可以用 instruction 和少量示例快速完成分类，但答案对 instruction phrasing、示例和 label token 很敏感；微调更稳定，却需要更新整个模型、显存和更多数据。本文提出 In-Context Probing（ICP）：仍用 instruction 把输入上下文化，但不让 LLM 直接 decode label，而是在最后一层 representation 上训练一个很小的 classifier。

## Method / Framework
给 encoder-decoder FLAN-T5 输入 instruction+input，取最后一层所有 token 的 contextualized representations。ICP-REG 对序列 representation 做 pooling 后接 logistic regression；ICP-ATT 用单层单头 attention 聚合 token，再接分类头。只有 probe 训练，LLM 权重冻结，因此 instruction 提供语义上下文，监督信号直接训练轻量 label head。

实验覆盖 FLAN-T5 Small/Base/Large/XL/XXL，以及 Medical Questions Pairs (MQP)、TweetEval hate speech 和 Climate-FEVER 三个 sentence classification 任务；测试不同 instruction、训练样本数、模型规模、ICL calibration、probe 结构和 full fine-tuning。

## Baselines / Comparisons
对照为直接 ICL decoding、content-free calibrated ICL、ICP-REG、ICP-ATT 和 prompt-based fine-tuning。指标是 macro-F1 及不同 instruction 的均值/标准差；finetuning 与 ICP 都用 200 个训练样本做公平比较，另画出 40/80/100 样本的 sample-efficiency 曲线。

## Experiments / Findings
ICP 在所有任务和模型规模上比 ICL 更稳健，模型较小时常优于 ICL，模型较大时达到 competitive performance。一个明确的表 2 结果是 FLAN-XXL 上 ICP-ATT 67M 参数在 MQP/TWEETEVAL/CLIMATE 达到 89.22/71.64/53.92，ICP-REG 仅 10K 参数却达到 89.95/72.95/59.34，说明复杂 attention probe 并不必然带来收益。

训练样本很少时 ICP 也能达到 decoding baseline：MQP 约 40 个样本即可，其他任务约 80 个样本；与 full fine-tuning 相比，probe 最多少约 6 个数量级的可训练参数，却在多个任务/模型规模上竞争力相当。其核心收益是把“instruction 影响 representation”与“label decoding”分离，减少 verbalizer 和提示措辞的不稳定。

## Ablation / Error Analysis
ICP-REG vs ICP-ATT 消融显示加 attention 参数没有稳定收益，低复杂度 logistic probe 更划算。Calibration 只能部分修复 ICL 的 label prior，不能消除 phrasing sensitivity；训练 instruction 改变时 ICP 与 fine-tuning 的标准差都较低，而直接 ICL 波动更大。

失败情形包括极小模型 representation 不含足够任务信息、instruction 与 label 语义不匹配、类别不平衡和 decoder-only 架构尚未充分测试。ICP 的稳定性也依赖所选 instruction 仍能把任务语义注入 representation，并不意味着 probe 提供了模型内部任务机制的因果解释。

## Limitations
实验主要是 FLAN-T5 encoder-decoder，作者没有在真正超大或 decoder-only LLM 上系统验证；模型规模上限也受可访问性限制。ICP 需要少量标注数据和 representation access，不能直接用于闭源 API；它优化分类性能而非生成式回答或通用解释。

## 两句话总结
ICP 保留 instruction 的上下文学习作用，但把最后层 representation 交给轻量 probe 预测标签，从而显著降低 ICL 对 prompt wording 的敏感性。实验表明在低资源和小模型场景它可胜过直接 ICL、接近微调，而 10K 参数的 logistic probe 已常常优于更复杂 attention probe。

## Evidence note
已逐段读取本地 PDF，核对了 FLAN-T5、MQP/TWEETEVAL/CLIMATE、ICP-REG/ATT、ICL/calibration/fine-tuning 对照、表 2 数值、样本效率和 decoder-only 限制。
