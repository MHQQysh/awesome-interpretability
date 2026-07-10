# 207. Interleaved-Modal Chain-of-Thought

- **Authors:** Jun Gao, Yongqi Li, Ziqiang Cao, Wenjie Li
- **Venue:** CVPR 2025
- **Paper:** https://doi.org/10.1109/CVPR52734.2025.01818
- **Tags:** Multimodal CoT, Visual Rationale, Attention-driven Selection, Interpretability

## Introduction

文本 CoT 能展示语言推理，但 VLM 的纯文本 rationale 往往无法指出图像中的细粒度证据，导致解释与视觉输入脱节。ICoT 要让每一步推理都包含视觉和文本成对 rationale，同时又不要求重新训练一个能自由生成图像 patch 的 VLM。

## Method / Framework

ICoT 产生交错的 visual-text reasoning steps；Attention-driven Selection (ADS) 读取 VLM attention map，在每个 reasoning signal 后选择最相关的输入 image patches 并插入后续上下文。ADS 无参数、即插即用，只依赖模型已有 attention；作者在 Chameleon-7B 的离散 vokens 和 Qwen2-VL-7B-Instruct 的 dense visual features 上验证。

## Baselines / Comparisons

实验覆盖 M3CoT、ScienceQA 和 LLaVA-W，在 Chameleon-7B/Qwen2-VL-7B 上比较已有 multimodal CoT、SCAFFOLD 等方法，并保持 zero-shot/one-shot 与近似 query cost。M3CoT、ScienceQA 用 accuracy，LLaVA-W 用 ROUGE-L，同时做 patch 数量、KV-cache 复制和 visual token 选择消融。

## Experiments / Findings

- 表中 ICoT 在 Chameleon/Qwen2-VL 设置达到 44.1、56.8、34.2、46.0、65.4、35.7 等任务分数，相对之前最好 baseline 的提升最高约 5.3%；论文摘要报告跨三个 benchmark 最大约 14% 的相对改善。
- 交错 patch 让 rationale 不再只是语言模型对图像的事后描述，定性结果能显示模型在每一步关注了哪个区域；这也是解释性收益而不只是 accuracy 增益。
- ADS 不需要参数训练，额外延迟很小，但 attention map 的存储和 patch selection 数量会影响性能；固定选择数并非所有图像都合适。

## Ablation / Error Analysis

消融 visual selection、signal token、patch 数、纯文本 rationale 和 KV-cache 复制。没有 ADS 时文本 CoT 容易引用无关区域；patch 太少会漏掉组合证据，太多则增加噪声和上下文成本。不同 VLM 的 tokenization、attention 实现和视觉 patch 大小也会改变最优选择粒度。

## Limitations

ICoT 仍把 attention 当作选择证据的 proxy，不证明模型真正因果使用了被选 patch；固定 patch 数和 eager attention 会带来显存/实现开销。实验只覆盖两个 backbone 和三个 benchmark，训练型、多语言、长视频和开放式视觉生成还未验证。

## 两句话总结

ICoT 用 attention-driven patch selection 把 VLM 的文本 CoT 改成视觉-文本交错的推理轨迹，使每步语言理由都有可检查的图像证据。它在多个 benchmark 上改善性能和可读性，但 attention 代理、固定 patch 数和额外显存仍是实际限制。
