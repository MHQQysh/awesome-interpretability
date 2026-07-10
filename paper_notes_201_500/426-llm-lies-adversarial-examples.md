# 426. LLM Lies: Hallucinations are not Bugs, but Features as Adversarial Examples

- **Authors:** Jia-Yu Yao, Kun-Peng Ning, Zhen-Hui Liu, Mu-Nan Ning, Yu-Yang Liu, Li Yuan
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2310.01469
- **Tags:** `hallucination` `adversarial-examples` `reasoning` `robustness`

## Introduction
LLM hallucination 常被视为 bug，但本文提出另一种视角：某些“会撒谎/会生成错事实”的输出可以由 adversarial prompt 诱导，类似模型决策边界上的 adversarial examples。研究目标是理解 hallucination 对输入扰动的敏感性，而不是只在自然 prompt 上测平均错误率。

## Method / Framework
作者构造/收集 factual questions 与 adversarial prompts，比较 GPT-3.5、LLaMA、PaLM 等在普通和对抗条件下的回答，分析 prompt、context、reasoning 和答案 confidence 的变化。检测重点是输出事实、与外部知识一致性和攻击成功率。

## Baselines / Comparisons
比较 clean prompt、人工 adversarial prompt、自动改写/诱导 prompt、不同模型和不同 decoding/knowledge setting。评价包括 accuracy、hallucination rate、攻击 transferability 和拒答/校准行为。

## Experiments / Findings
模型在自然问题上看似 knowledgeable，但局部输入变化可以显著改变答案并诱发 confident false claims；这种错误的可控性更接近 adversarial vulnerability，而不是随机噪声。不同模型对 prompt 类型和知识域的敏感性不同，单一 hallucination benchmark 不能覆盖攻击面。

## Ablation / Error Analysis
提示词位置、语义改写、上下文、temperature 和模型大小是主要消融。错误来源包括先验冲突、问题 framing、过度遵循上下文和缺乏外部 verification；攻击触发的错误不一定在普通 perplexity 或 confidence 中明显。

## Limitations
adversarial prompt 集合和 hallucination 定义有限，不能证明所有幻觉都是 adversarial examples。攻击成功率依赖模型版本和 prompt engineering；没有完整的内部机制定位或通用防御。

## 两句话总结
本文把 LLM hallucination 视为可由输入扰动诱发的 adversarial vulnerability，展示 confident false answers 不是单纯随机 bug。它提醒评测必须加入系统化 prompt attacks、transfer 和 robustness，而不是只报告 clean benchmark factuality。

## Evidence note
已读取 arXiv 2310.01469 v3 本地 PDF 的 adversarial hallucination framing、模型/提示对比和结论。
