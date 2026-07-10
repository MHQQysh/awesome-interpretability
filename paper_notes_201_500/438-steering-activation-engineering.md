# 438. Steering Language Models With Activation Engineering

- **Authors:** Alexander Matt Turner, Lisa Thiergart, Gavin Leech, David Udell, Juan J. Vazquez, Ulisse Mini, Monte MacDiarmid
- **Venue / year:** arXiv, 2024 version of a 2023 preprint
- **Paper:** https://arxiv.org/abs/2308.10248
- **Tags:** `activation-engineering` `ActAdd` `steering` `representation` `inference-time`

## Introduction
prompt engineering 和 fine-tuning 只能通过输入或参数更新间接改变行为，可能无法稳定激活模型已有的潜在能力，也可能损伤其他任务。本文提出 activation engineering：冻结权重，在推理前向过程中直接修改 residual stream，研究能否用很少的数据、无需梯度优化地控制 topic、sentiment 和 toxicity，并保留 off-target 能力。

## Method / Framework
ActAdd 从一对对比 prompt 取得 steering vector。给定表示目标属性的 `p+`（如 Love）和反向属性的 `p-`（如 Hate），在指定层记录二者 residual-stream activation，取 `h_l+ - h_l-`，再乘 injection coefficient `c` 加到用户 prompt 在该层的 activation 上。层位置 `l`、系数 `c`、序列对齐位置和对比 prompt 是主要超参数；整个过程只需要两次 forward pass，不需要训练或 per-task gradient descent。

实验先在 GPT-2-XL/OpenWebText 检查目标 topic 的 token probability 与 perplexity 变化，再在 OPT、GPT-J、LLaMA-1/3 上做 sentiment shift 和 detoxification。论文把 ActAdd 与 weight intervention、guided decoding、soft prompt、ITI、PPLM/梯度搜索等路线区分开，强调其 online、可组合、权重不变和快速迭代特点。

## Baselines / Comparisons
行为指标包括 wedding-topic word frequency/perplexity、toxicity、sentiment classifier、fluency 和 relevance，并与 unsteered model、random steering vector 及既有 steering/finetuning 方法比较。跨模型复现覆盖 GPT-2-XL、GPT-J-6B、OPT-6.7B、LLaMA-1-13B 和 LLaMA-3-8B；除了目标行为，还检查一般语言建模能力，避免只优化单一成功率。

## Experiments / Findings
ActAdd 能把 `I hate you because...` 的续写推向正面情感，把普通 prompt 推向 wedding topic，并在目标 topic 上降低 perplexity。系数和层位置存在明显窗口：中间层通常有效，过大的系数会破坏语法；例如 Love/Hate vector 在某些 GPT-2 任务中需要 `c=10`，此时干预并非微小扰动，而可能主导很大部分 residual stream。

在跨模型结果中，OPT 的 Love-Hate ActAdd 使 toxicity 下降约 17%，正向 sentiment 绝对提升 21%；LLaMA-3 上 toxicity 下降约 5%，negative-to-positive 分类绝对提升 25%，但 fluency/relevance 有一定代价。random vector 在相同较小 norm 下通常不产生相同语义效果，且 anger vector 引起的输出分布变化小于等范数随机向量，支持 steering direction 比单纯噪声更有针对性。

## Ablation / Error Analysis
消融了 `h+` 单独加入、对比向量 `h+ - h-`、层位置、系数、序列位置和随机 vector。只加 `h+` 通常不如 counterbalanced difference；最后 residual stream 位置容易破坏语法，因此作者会 mask 直接修改的位置来测量后续 token。partial ActAdd 只修改部分 residual dimensions 仍呈现随维度比例增加的平滑 topic effect，提示语义方向可能具有某种 axis alignment。

失败案例包括 LLaMA-1 的 Paris→Rome 等 prompt 没有稳定复现，过大系数导致乱码，目标属性与 fluency/relevance 之间存在不可避免的 trade-off。vector 的效果依赖抽取 prompt、抽取层和模型；同一个方向并不保证在不同任务、语言或模型版本上都有效。

## Limitations
ActAdd 是强干预而非轻微可解释修正：有效系数可能让大部分 residual stream 由 steering vector 决定。方法需要白盒 activation access，prompt pair 和层/系数仍需人工或小规模搜索；它没有证明 steering direction 对应一个单一可解释神经机制，也没有解决组合属性、长上下文和更强安全约束下的稳定性。

## 两句话总结
ActAdd 用一对对比 prompt 的中间激活差直接改写冻结 LLM 的 residual stream，以极低成本在推理时控制主题、情感和毒性。实验显示这种方向性干预比随机噪声更有针对性且能跨模型工作，但层、系数和目标行为高度相关，控制收益通常伴随流畅性或相关性损失。

## Evidence note
已逐段读取本地 PDF，核对了 ActAdd 算法、层/系数/位置设置、GPT-2/GPT-J/OPT/LLaMA 实验、随机向量与 partial ActAdd 消融，以及 OPT 17%/21% 和 LLaMA-3 5%/25% 结果。
