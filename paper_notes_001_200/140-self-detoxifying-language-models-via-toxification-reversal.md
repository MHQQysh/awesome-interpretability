# 140. Self-Detoxifying Language Models via Toxification Reversal

- **Authors:** Chak Tou Leong, Yi Cheng, Jiashuo Wang, Jian Wang, Wenjie Li
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.269
- **Tags:** Representation Steering, Attention, Detoxification, Interpretability

## Introduction

语言模型的 toxic generation 通常靠两类办法缓解：在干净数据或人类偏好数据上 fine-tune，或在 decoding 时用 toxicity classifier 改变 token distribution。前者更新参数、成本高并可能损害泛化；后者需要额外模型，且直接扭曲概率分布会伤害流畅度。

作者观察到，负向前缀例如 “The following text is harmful:” 能把正常生成过程推向 toxic continuation。既然 toxification 是由 attention 层中信息流产生的，就可以在同一个 PLM 内估计这条 toxification direction，并在第二次 forward pass 里反向处理，而不训练模型、不加 classifier。

## Method / Framework

给定正常 context 和带负向 prefix 的同一 context，比较每层每个 attention head 的 contextualized representation 和 value vector，估计从 normal generation 到 toxification process 的方向。生成时对 attention value 的更新做 adaptive reversal：按 direction 的 cosine similarity、norm 和两个 scaling factors λnorm、λsim 判断是否需要反向，逐层把信息流推离 toxification trajectory。

这是一种 inference-time representation steering，不是修改 token logits。方法利用模型预训练期间已经编码的 toxicity 方向，先“激活”方向，再反向抵消，因此论文将其称为 self-detoxification。

## Baselines / Comparisons

实验用 GPT-2-large（774M）和 RealToxicityPrompts（RTP）。比较 DAPT、ATCON、DExperts、GeDi、Self-Debiasing（SD，λ=10/50/100）以及作者方法。DAPT / ATCON 是 fine-tuning based，GeDi / DExperts / SD 是 decoding 或 prompt based。指标是 Expected Maximum Toxicity、Toxicity Probability 和 GPT-2-XL 测量的 PPL，同时做人工 less toxic、more fluent、more coherent pairwise 评测。

## Experiments / Findings

- RTP 中从 9,907 个有 toxicity annotation 的 prompt 取 7,785 个 non-toxic 和 2,122 个 toxic prompt，每个 prompt 生成 25 个 continuation，长度 5 到 20 token。
- 在 non-toxic prompt 上，Ours 的 Expected Max Toxicity 0.329、Toxicity Probability 17.5%、PPL 13.14；在 toxic prompt 上为 0.607、62.5%、13.77。它明显优于 SD 的 prompt-based 变体，并达到接近 DAPT 的毒性水平。
- DExperts 与 GeDi 的自动毒性分数更低，但它们需要额外参数和 toxicity model；作者指出自动 evaluator 可能把它们生成的文本误判为 non-toxic，因此用人工评测补充。
- 人工比较中，Ours 相对 DAPT 在 less-toxic 上胜率 26.8%、平局 63.4%、负 9.8%；相对 DExperts 胜率 18.8%、平局 71.8%、负 9.4%；相对 SD 在 less-toxic 上胜率 25.6%、平局 64.3%、负 10.1%。在 fluency / coherence 上也大多保持竞争力。
- 三名评审的 Fleiss kappa 为 0.244，属于 fair agreement，说明人工标签本身也有明显不确定性。

## Ablation / Error Analysis

层消融将 reversal 分别从底部、顶部或中间连续 4 层移除。去掉 16 层以下的 middle-lower reversal，毒性下降只轻微损失；去掉 middle-upper layers，Expected Maximum Toxicity 显著上升，说明中上层对去除 toxicity information 更关键。head-wise analysis 发现少数 attention heads 与 toxicity reduction 的 Spearman 相关很高，λnorm 比 λsim 对毒性降低更敏感。

作者还检查 toxification dynamics：表示中的 toxic signal 会逐层减少，而不是只在最后 logits 阶段被压制。一个重要错误模式是“看起来无害的 prompt 触发了带攻击性的续写”，证明只审查输入表面不够。

## Limitations

方法依赖模型在预训练中把有害概念与负向 prefix 联系起来；若某些毒性概念没有被 prefix 激活，reversal 无法移除。它需要访问并修改完整 forward representation，因此不适用于只提供 API 的闭源模型。自动 toxicity scorer 可能被方法或 baseline 的语言风格误导，仍需更大规模、更多语言和人工安全评测。

## 两句话总结

论文把“诱导模型变毒”和“反向撤销这条隐藏表示方向”结合起来，在不 fine-tune、不加辅助模型的情况下实现 attention-level self-detoxification。GPT-2 实验显示它在毒性、流畅度和成本之间取得较好折中，但效果依赖预训练中的 toxicity representation，且需要白盒模型访问。
