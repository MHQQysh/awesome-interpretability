# 228. Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet

- **Authors:** Adly Templeton, Tom Conerly, Jonathan Marcus et al.
- **Venue:** arXiv 2026
- **Paper:** https://arxiv.org/abs/2605.29358
- **Tags:** Sparse Autoencoder, Claude 3 Sonnet, Monosemanticity, Steering

## Introduction

SAE dictionary learning 已在小 Transformer 上提取可读 feature，但是否能扩展到 production-scale Claude 3 Sonnet、是否能捕捉抽象/安全概念并支持干预仍是开放问题。论文以中间层 residual stream 为对象，研究 dictionary size scaling 与 feature interpretability、geometry、causal steering 的关系。

## Method / Framework

作者训练最多 34 million features 的 sparse autoencoders，用 scaling laws 选择 dictionary/稀疏超参，并通过 feature activation examples、自动/人工解释与 feature geometry 分析质量。对选中的 feature 做 activation steering，检查 manipulation 是否按语义改变 model output。

## Baselines / Comparisons

比较不同 dictionary size、feature frequency、稀疏性与 reconstruction/interpretability trade-off；案例覆盖多语言、图像描述、实体/地点、抽象概念、代码错误，以及 deception、power-seeking、sycophancy、bias 等安全相关 feature。指标包括 feature coherence、几何结构、可解释性和 steering 后行为变化。

## Experiments / Findings

- 大模型中仍可提取可读 features，且 feature 是 multilingual、multimodal 的：即使 SAE 仅在 text 上训练，也会对图像相关输入响应，并同时覆盖具体实例和抽象讨论。
- dictionary scaling 遵循与概念频率相关的规律：常见概念需要更大 dictionary 才能拆开，rare feature 可能在不同规模下被 split/absorb；这把 SAE 超参从经验调节变成可研究的 scaling law。
- 安全相关 feature 的 activation manipulation 会因果影响输出，证明 monosemantic feature 不只是标签化，也能作为行为 steering 接口；作者同时强调 feature dictionary 不完整、评估仍不严格。

## Ablation / Error Analysis

消融 dictionary size、feature frequency、SAE sparsity、层位置和 steering strength。过大 dictionary 增加 feature splitting，过小会把多个概念压进一个 feature；自动解释可能把相关性错当语义，steering 过强会引入语言质量/主题副作用。

## Limitations

Claude 3 Sonnet 是闭源 production model，feature extraction、复现和完整词典受访问限制；研究仍只覆盖部分层、输入与 safety concepts。feature 的“可解释”与“真实因果功能”之间仍有差距，不能把个别 steering 成功当作完整 circuit 发现。

## 两句话总结

论文把 SAE monosemanticity 扩展到 Claude 3 Sonnet 的千万级 feature dictionary，发现可解释概念能跨语言、图像和抽象语境，并可用于安全行为 steering。规模化带来更细的 feature 也带来 splitting、词典不完整和评估不足，完整机制仍未建立。
