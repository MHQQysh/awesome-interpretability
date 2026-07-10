# 281. MONET: Mixture of Monosemantic Experts for Transformers

- **Authors:** Jong-Ho Park, Young Jin Ahn, Kee-Eung Kim, Jaewoo Kang
- **Venue / year:** ICLR 2025
- **Paper:** https://arxiv.org/abs/2412.04139
- **Tags:** Monosemanticity, Mixture-of-Experts, Knowledge Manipulation, Toxicity

## Introduction

SAE 通常在预训练后 post-hoc 重构 activation，重构误差会破坏模型性能，且 feature 对模型功能的重要性不一定被保留。MONET 的问题是能否把 sparse dictionary learning 直接融入端到端 MoE pretraining，使 expert 本身更接近 monosemantic feature，同时保持语言模型能力和可控知识操作。

## Method / Framework

MONET 用 novel expert decomposition 将每层的 hidden computation 拆成大量稀疏 experts，支持最多 262,144 experts/layer；总参数量随 expert 数近似平方根扩展，而非线性增长。它提供 horizontal/vertical decomposition，使用 router 将 token 分配给少量 expert，并在端到端训练中让专家学习相对互斥的知识。训练后可以按 domain、language 或 toxicity 对专家 mask/edit，实现 domain unlearning 和 steering。

## Baselines / Comparisons

比较包括参数匹配的 dense LLaMA、标准 OLMoE、Gemma 2 + Gemma Scope SAE、LLaMA MLP neuron selection，以及 MONET-HD/VD。评估覆盖 language modeling、0/5-shot WG、OBQA、HellaSwag、CSQA、MULTIPL-E programming pass@100、知识删除后的目标/非目标性能和 toxic content control。

## Experiments / Findings

MONET 在 850M、1.4B、4.1B 三个规模上保持竞争力，且随着规模增大 0/5-shot 性能上升；vertical decomposition 通常优于 horizontal。专家之间的知识分布更互斥，能把被 mask 的 target language/domain 的 pass@100 下降限制在目标范围，非目标语言保持得更好。相比 Gemma Scope，post-hoc SAE reconstruction 更容易引入开放任务不稳定；标准 OLMoE/LLaMA 的 polysemantic units 难以精确定位领域知识。

## Ablation / Error Analysis

HD/VD、expert 数量、routing 与 domain assignment 是主要消融。MONET 的专家虽然比 LLaMA 平均约 2.2% specialized neurons 更容易定位，但专家 specialization 仍不等同于完美单义；过强 mask 会损害共享能力，过弱 mask 则残留知识。SAE baseline 的 reconstruction error 会级联到后续层，解释了 control/性能 trade-off。

## Limitations

端到端训练成本高，262K experts 的分析和路由管理复杂；“专家互斥”是统计/任务层面的，不保证每个 expert 真正只含一个概念。domain unlearning 仍受测试集、路由准则和知识重叠影响，不能等同于不可恢复的 unlearning。

## 两句话总结

MONET 把稀疏字典学习从 post-hoc SAE 变成端到端 Mixture-of-Experts，使专家更单义并支持按领域/语言/毒性操作知识。它比重构式 SAE 更能维持通用性能和局部编辑，但训练成本、概念重叠和真正遗忘仍是问题。

## Evidence note

本笔记逐段核对 ICLR/arXiv PDF 的 architecture、Table 2/3、domain unlearning、scaling 和 ablation；数值只保留正文明确报告的结果。

