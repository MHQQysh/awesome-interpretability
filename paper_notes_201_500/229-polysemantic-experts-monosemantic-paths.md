# 229. Polysemantic Experts, Monosemantic Paths: Routing as Control in MoEs

- **Authors:** Charles Ye, Bo Yuan, Lee Sharkey
- **Venue:** ICLR 2026 Workshop
- **Paper:** https://arxiv.org/abs/2604.17837
- **Tags:** Mixture of Experts, Routing, Monosemanticity, Control Subspace

## Introduction

MoE 中单个 expert 往往是 polysemantic，难以解释为什么相同 token 在不同上下文走不同路径；但路由本身可能是模型控制流的低带宽通道。论文的问题是能否把 residual hidden state 分解为“驱动 router 的 control signal”和对 router 不可见的 content channel，从路径而非单 expert 理解 MoE。

## Method / Framework

作者提出 parameter-free decomposition，把每层 hidden state 投影成 router-visible control 与 orthogonal router-blind content。随后对六种 MoE 架构分析路由 probe、跨层 control rotation、expert path clustering 和 token trajectory，检查 path 是否比单个 expert 更 monosemantic，以及 top dimensions 是否足以恢复 routing。

## Baselines / Comparisons

比较完整 hidden representation、control channel、content channel 和不同层/模型的 routing probe；模型覆盖 OlMoE、Granite-4.0-Tiny、DSv2-Lite、Qwen3-30B-A3B、gpt-oss-20b、GLM-4.5-Air 等。指标包括 probe accuracy、feature/trajectory coherence、跨语言/表面形式聚类和 routing dimension sparsity。

## Experiments / Findings

- control channel 比 full representation 更 monosemantic，content channel 更多保留语言、token identity、position 等表面信息；这说明专家的 polysemanticity 可能来自共享内容通道而非路由控制本身。
- 同一 token（如冒号）在 type annotation、introductory colon、time separator 等语境中进入不同路径，expert path 比单 expert 更能按语义功能聚类。
- routing probe accuracy 在只保留最高幅度 2--5% hidden dimensions 后迅速饱和，支持 routing 是低秩/低带宽控制信号的假设；control direction 还会随层旋转，形成跨层组合 specialization。

## Ablation / Error Analysis

消融 control/content projection、top-k dimensions、层深和不同 MoE。正交分解的基底选择会影响“不可见”定义，probe 可恢复 routing 不等于模型只使用这些维度；专家 path 仍可能混合多个语义，尤其在短上下文或路由负载均衡干预下。

## Limitations

这是 workshop/preprint 工作，模型版本、数据和评估尚有限，尚未证明路径语义能直接预测生成行为或支持安全 steering。parameter-free projection 是分析选择而非训练中真实电路，跨架构泛化与对路由的因果干预仍需实验。

## 两句话总结

论文把 MoE hidden state 拆成 router control 与 content 两个通道，发现单 expert 虽 polysemantic，但跨层 expert paths 更接近 monosemantic 控制单元。路由信息集中在少量维度是重要线索，但 probe 和几何分解仍需要因果验证。
