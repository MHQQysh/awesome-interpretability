# 470. Explaining Black Box Text Modules in Natural Language with Language Models

- **Authors:** Chandan Singh, Aliyah R. Hsu, Richard Antonello, Shailee Jain, Alexander G. Huth, Bin Yu, Jianfeng Gao
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2305.09863](https://arxiv.org/abs/2305.09863)
- **Tags:** `black-box-explanation` `natural-language-explanation` `BERT` `neuroscience`

## Introduction
很多 text module 只能通过输入/输出访问，既没有权重也没有可读 feature；论文问能否只用 black-box response 自动得到自然语言 selectivity 说明，并给 explanation reliability score。目标覆盖 synthetic module、BERT 内部 factor 和 fMRI voxel response。

## Method / Framework
SASC（Summarize and Score）两步走：从 reference corpus 提取 n-grams，按模块响应取 top n-grams，让 helper LLM 生成多个候选说明；再让 helper LLM 根据每个候选生成 positive/negative synthetic text，以 `E[f(Text+) - f(Text-)]` 的标准差单位作为 score，选择最高候选。默认 GPT-3 helper，30 个 top-50 trigram，5 个候选，每候选 20 个 synthetic sentence。

## Baselines / Comparisons
synthetic module 上比较只做 ngram summarization、gradient-based explanation 和 LDA topic modeling；BERT factor 上与既有 human explanation 比较；下游用 sparse logistic regression 选出 emotion、AG News、SST2 相关 modules，再人工判断 explanation relevance。

## Experiments / Findings
54 个 synthetic modules 上，Default SASC accuracy 0.883±0.03、BERT score 0.712，baseline 为 0.753/0.622；restricted corpus 为 0.667，noisy module 为 0.679，仍高于 baseline。gradient/topic accuracy 仅 0.093/0.111。BERT 1,500 factors 中，SASC explanation score 高于 human explanation 的比例 61%，均值 1.6sigma 对 1.0sigma；下游 explanation relevance 为 Emotion 0.35、AG News 0.96、SST2 0.44。

SASC 还对 1,500 个拟合良好的 fMRI voxel module 生成说明，发现相较 BERT，脑 voxel 说明更偏 social/family/communication 与 perceptual/action 概念，提供比粗粒度 emotion label 更细的 selectivity hypotheses。

## Ablation / Error Analysis
限制 corpus、给 module response 加噪、改用 bigram/4-gram 或 LLaMA-2 helper 是主要敏感性检查；helper 模型能力越强，说明恢复通常越好。SASC 失败时常是 ngram 无法表达模块模式，或 LLM 把 `ungrammatical` 泛化成“language”；分数越高的候选通常更可靠，但这是 synthetic scoring consistency，不是 causal proof。

## Limitations
SASC 假设 module 能被一句自然语言概括，只描述最大响应而非全行为，且依赖 helper LLM 忠实总结/生成。top n-gram corpus 决定候选空间，LLM hallucination 和 explanation score 可能造成自洽但错误的说明，仍需人工审查。

## 两句话总结
SASC 用“top n-gram 总结 + 合成文本打分”把黑盒 text module 转成带可靠性分数的自然语言解释，在 synthetic、BERT 和 fMRI 上优于简单摘要/gradient/topic baseline。它扩大了自动解释的覆盖面，但只说明 selectivity 的高响应区域，不保证解释覆盖全函数或揭示因果机制。

## Evidence note
已读取 SASC 两步算法、synthetic 三种设置、全部表格、BERT factor/downstream relevance 和 fMRI voxel 分析。
