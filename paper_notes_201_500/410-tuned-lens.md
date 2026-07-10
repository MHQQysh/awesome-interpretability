# 410. Eliciting Latent Predictions from Transformers with the Tuned Lens

- **Authors:** Nora Belrose, Igor Ostrovsky, Lev McKinney, Zach Furman, Logan Smith, Danny Halawi, Stella Biderman, Jacob Steinhardt
- **Venue / year:** ICLR, 2024
- **Paper:** https://arxiv.org/abs/2303.08112
- **Tags:** `tuned-lens` `logit-lens` `layerwise-probing` `causal-interpretability`

## Introduction
Logit lens 直接把中间层 hidden state 过 final unembedding，观察模型逐层形成的预测，但早期层的表示坐标与 final layer 不对齐，导致 raw logit lens 读出很差。Tuned Lens 学习每层的 affine translator，把中间表示映射到最终表示空间，以更可靠地观察 prediction trajectory。

## Method / Framework
对每个 layer 训练一个轻量 tuned lens translator，使其输出分布逼近模型最终输出。它仍冻结主模型，训练成本远低于重新训练 LM；可用于逐层 latent prediction、confidence、anomaly/prompt injection detection、example difficulty 和模型 stitching。

作者还做 causal validation：寻找 tuned lens 重要方向并干预 hidden state，检查该方向是否也改变 final model output；比较 tuned lens 与 logit lens 的 stimulus-response alignment 和 interpretability。

## Baselines / Comparisons
主要 baseline 是 raw logit lens、final-layer prediction、中间层 linear probes 和随机矩阵。实验使用 Pythia 多规模、BLOOM、OPT 等模型，比较 perplexity/cross-entropy、layer transfer penalty、causal influence、prompt injection AUROC、difficulty correlation 和 toxicity/secret elicitation 应用。

## Experiments / Findings
Tuned lens 通常比 logit lens 更早、更准确地读出中间层预测，并能在相邻 layers transfer；它显示 prediction trajectory 不是简单单调 refinement，但在多数任务上中间层已经包含可用答案信息。tuned lens 用于 prompt-injection detection 的 AUROC 通常优于 raw logit lens，且能估计 prediction depth：需要更多层才收敛的样本往往更难。

因果实验发现 tuned lens 重要方向与 final output 的 causal influence 高相关，Pythia 410M 示例 Spearman rho 约 0.89；tuned lens 也常比 logit lens 更好地恢复早期 representation 的可读输出。

## Ablation / Error Analysis
translator layer、训练步数、保留/不保留 final layer、不同模型和 lens transfer 是主要消融。logit lens 可能受 outlier dimensions、tokenization 和未对齐 unembedding 影响；tuned lens 虽改善读出，也可能学会 spurious features，因此作者用 intervention 检查其 causal sensitivity。

局限性案例包括某些模型/层 tuned lens 仍无法预测 final output，prompt injection detection 的阈值和任务分布敏感，以及用 lens 解释 representation 不等于解释模型真实 computation。

## Limitations
每层都要训练 translator，计算和存储成本随层数增加；跨模型 transfer 有 penalty。tuned lens 是 probe/translator，不是完整 circuit；它可以提供有用的 layerwise monitor，但不能保证自然语言解释 faithful。某些 toy/secret/toxicity 应用与实际部署分布仍有差距。

## 两句话总结
Tuned Lens 为每层学习一个轻量映射，把中间 hidden state 转到 final output space，比 raw logit lens 更可靠地观察 Transformer 的预测轨迹和难度。它通过因果方向验证和 prompt-injection 应用展示了 probe 的价值，但 layerwise readout 仍是解释工具，不是模型全部机制。

## Evidence note
已读取 arXiv 2303.08112 本地 PDF 的 tuned/logit lens 方法、层迁移、因果验证、异常检测、prediction depth 与 limitations。
