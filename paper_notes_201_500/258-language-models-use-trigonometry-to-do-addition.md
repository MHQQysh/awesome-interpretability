# 258. Language Models Use Trigonometry to Do Addition

- **Authors:** Subhash Kantamneni, Max Tegmark
- **Venue / year:** arXiv 2025; ICLR 2025 workshop version
- **Paper:** https://arxiv.org/abs/2502.00873
- **Tags:** Mechanistic Interpretability, Arithmetic Circuit, Helical Representation, Activation Patching

## Introduction

LLM 能做简单算术，但“能答对”并不说明模型内部实现了什么算法。论文研究 GPT-J 6B、Pythia-6.9B 和 Llama 3.1-8B 如何计算 a+b（主要取 a,b∈[0,99]），试图把 representation、circuit 和 causal computation 连起来，而不是只找到一个能线性 probe 出数字的方向。

## Method / Framework

作者先分析 residual stream 的 PCA 和 Fourier spectrum，发现数字既有随数值变化的近似线性成分，也有周期 T=2、5、10、100 的 Fourier 成分，于是用一个共享线性方向加多个正余弦对的 generalized helix 拟合表示。随后用 activation patching：把 clean prompt 的 a 表示 patch 到 corrupted prompt，比较答案 token 的 logit difference，并将 helix、circle、PCA、polynomial fit 与真实 layer patch 对照。最后在 GPT-J 内部做 attention-head/MLP/neuron total effect、path patching 和 preactivation Fourier 拟合，提出 Clock algorithm。

## Baselines / Comparisons

表示拟合的 baseline 包括真实 residual patch、前 2k+1 个 PCA 分量、仅 Fourier circle 和 polynomial basis；任务级别还比较 addition、subtraction、integer division、乘 1.5、mod 2 与 x-a=0。电路分析不靠单个 probe，而用 attention/MLP/neuron 的 patching 与 mean ablation，测试拟合表示是否保留真正能恢复答案的因果信息。

## Experiments / Findings

- GPT-J 的 helix 使用 T=[2,5,10,100]，9 个 basis parameters 在 addition 上的 patch logit difference 为 7.21，真实 layer patch 为 8.34，PCA 为 6.13，circle 为 6.83，polynomial 仅 3.09。
- 在多任务表中，helix 的最大 causal logit difference 为：a+ b 7.21、a-23 7.05、a//5 4.55、a*1.5 5.16、a mod 2 1.11、x-a=0 4.56；它并非所有任务都优于 PCA，说明 helix 是强表示解释但不是完整算术理论。
- GPT-J 的 Clock circuit 大致分工是：少数 layer 9-14 attention heads 把 a/b helix 搬到最后 token；MLP 14-18 组合成 helix(a+b)；MLP 19-27 与少数 attention heads 读取它并写入 logits。约 20 个 attention heads 能达到全部 attention patch 效果的 83.9%，11 个关键 MLP 达到约 95%。

## Ablation / Error Analysis

随机打乱数字训练顺序的 helix 不具备同样 causal power，train/test split 也显示不是简单过拟合。只保留约 1% 的高效 neurons 仍能让 GPT-J 完成约 80% prompts；对这些 neuron preactivation 做 helix 拟合，保留约 75% 的真实 neuron performance。错误主要来自不同层的表示混合、三位数在 100 处的不连续、模型对 division/multiplication 使用额外电路，以及作者尚未精确隔离 MLP 如何执行 cos(a+b) 的乘加。

## Limitations

结论最完整的是 GPT-J，Pythia/Llama 只提供较弱的 helical evidence；任务范围也主要是有限整数算术。研究没有完全反推出三角恒等式对应的权重级算法，不能把 Clock 图示当作所有 LLM 的通用算术机制。模型规模、训练语料和 tokenization 改变后，表示周期与电路可能不同。

## 两句话总结

论文发现 LLM 可把数字表示成带线性轴和多个周期相位的 generalized helix，并用 attention 搬运、MLP 组合、后续层读取的 Clock circuit 做加法。它用因果 patching 而非相关性 probe 支持了表示级解释，但 helix 对不同算术任务并不完整，精确权重机制和跨模型普适性仍未解决。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Problem Setup、Sections 4-5、Tables 1-3、patching 与 neuron analysis；数值来自正文表格和电路描述。

