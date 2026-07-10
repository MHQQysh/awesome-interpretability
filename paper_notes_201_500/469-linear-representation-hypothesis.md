# 469. The Linear Representation Hypothesis and the Geometry of Large Language Models

- **Authors:** Kiho Park, Yo Joong Choe, Victor Veitch
- **Venue / year:** ICML 2024
- **Paper:** [arXiv:2311.03658](https://arxiv.org/abs/2311.03658)
- **Tags:** `linear-representation` `geometry` `probing` `steering` `causal-inner-product`

## Introduction
“概念是向量方向”常被用于 probing、cosine similarity 和 activation steering，但 subspace、measurement、intervention 三种 linear representation 是否等价，以及该空间该用什么 inner product，并不清楚。论文用 counterfactual word pairs 把这三个概念统一起来。

## Method / Framework
作者定义 output/unembedding representation 与 input/context representation，提出 causal separability，并推导满足语义独立性的 causal inner product：在 LLaMA-2 上取 `M=Cov(gamma)^-1`（D=I），使 embedding/unembedding 概念表示统一。概念方向由 counterfactual pairs 的平均差向量估计；其一方面用于 linear probe，另一方面转换成 steering vector，测试对目标概念的干预是否不影响因果可分概念。

## Baselines / Comparisons
实验使用 LLaMA-2 7B、BATS 22 个概念、4 个语言翻译概念和 frequent/infrequent；比较 counterfactual pair 与 100K random word-pair projections，比较 causal inner product 与普通 Euclidean geometry，并进行 27 个概念的正交性 heatmap 和成对 steering。

## Experiments / Findings
除 `thing->part` 外，counterfactual differences 明显比随机差分更对齐到对应 concept direction；27 个概念在 causal inner product 下大多接近正交，并呈现按语义族的 block structure。方向可作为 probe：例如 French/Spanish context 的 French->Spanish direction 能区分目标语言，而 male/female direction 对无关概念不敏感。将 steering vector 加入 context 会提高 target concept 的 log-prob，同时基本保持 causally separable off-target concept 不变。

## Ablation / Error Analysis
Euclidean inner product 不能一般性保证 causal separability；概念方向质量受单 token 过滤、counterfactual pair 数量和语言相关性影响。thing->part 失败说明“线性概念”不是普遍真理；非零 block 也可能来自语言/语义相关而非完全独立概念。

## Limitations
理论依赖 causal separability 与随机词假设，inner product 并不唯一，D=I 是实验选择。实验主要是词级概念与 LLaMA-2，不能自动推出句法、长程推理或 SAE feature 都具有同样几何结构；steering 成功也不等于解释了模型的原始计算。

## 两句话总结
论文把 probing、概念子空间和 steering 统一为同一套 counterfactual/causal geometry，并说明 inner product 的选择决定“概念方向”是否有语义意义。LLaMA-2 实验支持许多词级概念近似线性且可干预，但存在概念失败、相关性和假设依赖。

## Evidence note
已读取 causal inner product 定理、27 概念投影/正交性、probe 与 intervention 实验及附录限制。
