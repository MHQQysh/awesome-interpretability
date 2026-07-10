# 476. From Language Modeling to Instruction Following

- **Authors:** Xuansheng Wu, Wenlin Yao, Jianshu Chen, Xiaoman Pan, Xiaoyang Wang, Ninghao Liu, Dong Yu
- **Venue / year:** arXiv 2023/2024
- **Paper:** [arXiv:2310.00492](https://arxiv.org/abs/2310.00492)
- **Tags:** `instruction-tuning` `attribution` `attention-heads` `feed-forward-interpretability`

## Introduction
instruction tuning 显著提高 helpfulness，但它究竟改变了预训练模型哪些内部机制并不清楚。论文从 input-output attribution、attention word-pair 和 feed-forward concept 三个层面比较 LLaMA/Mistral 与 Vicuna 等 tuned model。

## Method / Framework
作者提出归一化 pairwise importance，缓解不同 output token 的 score 不可比，再用 sparse density 汇总 instruction words 对整个 response 的贡献。attention head 通过 neuron-level word pairs 和 co-occurrence constraint 解释；FFN 视作 key-value memory，用 covariance PCA 的正交方向和 ChatGPT 描述其概念，比较 instruction verbs/用户任务概念的变化。

## Baselines / Comparisons
比较 pre-trained LLaMA vs instruction-tuned Vicuna、Mistral tuned/general 版本，并按 Self-Instruct、LIMA、MT-Bench 的 followed/unfollowed response 对照。head 分析还对比 instruction verbs 与 3000 个普通 verbs，FFN 对比不同 layer/family 的 concept distribution。

## Experiments / Findings
Vicuna instruction-word importance density 在 Self-Instruct/LIMA/MT-Bench 的 followed vs unfollowed 为 1.2283/0.8917、1.6173/1.2799、1.4584/0.9290；差异显著。Vicuna 对三数据集 instruction density 为 1.1302/1.5579/1.3440，均高于 LLaMA 0.9394/1.2683/1.1777。instruction tuning 让低层 attention 更常编码 write/create/classify 等 instruction verbs，并把 FFN 的预训练知识旋转到面向用户任务的概念。

## Ablation / Error Analysis
importance 与“是否按指令做”相关但不保证事实正确；案例显示模型可能表面遵循指令却没有理解意图。head word-pair overlap 随层深增加变化更大，Mistral 在更多层编码 instruction verbs；概念方向是统计/解释性分析，不是对某一 head 的行为做因果 knockout。

## Limitations
attribution 对自回归生成的归一化和 threshold 敏感，word-pair 仍受 polysemanticity 与 GloVe 相似度影响。instruction following 的人工标签、模型家族和 prompt 分布有限，不能把“更关注 instruction word”直接等同于 alignment 或 safety。

## 两句话总结
论文显示 instruction tuning 不只是改变输出风格，而是让模型持续读取 instruction words、重组 attention 的动词关系，并旋转 FFN knowledge toward user tasks。其解释框架把 behavior shift 连接到内部表示，但重要性与真正理解/忠实性仍需因果验证。

## Evidence note
已读取 attribution 定义、density 统计、attention word-pair 方法、FFN PCA/概念分析和 instruction verb 结果。
