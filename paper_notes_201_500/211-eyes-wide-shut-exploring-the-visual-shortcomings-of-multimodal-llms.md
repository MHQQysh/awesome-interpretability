# 211. Eyes Wide Shut? Exploring the Visual Shortcomings of Multimodal LLMs

- **Authors:** Shengbang Tong, Zhuang Liu, Yuexiang Zhai, Yi Ma, Yann LeCun, Saining Xie
- **Venue:** CVPR 2024
- **Paper:** https://doi.org/10.1109/CVPR52733.2024.00914
- **Tags:** MLLM Evaluation, Visual Grounding, CLIP, Vision Encoder

## Introduction

MLLM 在复杂问答上表现很强，但可能连“针朝上还是朝下”“杯子在桌面还是手里”“蝴蝶脚是否可见”这类简单视觉问题都答错。论文要把这种 visual shortcoming 与语言 hallucination 区分开，定位问题来自视觉编码器、语言模型还是二者对齐，而不是用总 benchmark 分数掩盖细粒度视觉失败。

## Method / Framework

作者构造 MMVP benchmark，用成对、简单且无歧义的问题测试方向、局部特征、状态、数量、位置关系、颜色、结构、文字和视角等 visual patterns；只有一对问题都正确才计分。再比较 CLIP 视觉编码器与 LLaVA/InstructBLIP 等 MLLM 的 pattern-level 表现，并提出 Mixture-of-Features：把 CLIP 与 DINOv2 等 vision-only SSL feature additive 或 interleaved 地接入 LLaVA。

## Baselines / Comparisons

评估 CLIP 系列不同 backbone/resolution、LLaVA-1.5、InstructBLIP、MiniGPT-4、Bard、Gemini 与 GPT-4V，并与人类用户研究比较。指标是 MMVP 平均准确率、各 visual pattern 准确率、CLIP-MLLM Pearson correlation，以及 MoF 对标准视觉问答/指令跟随的影响。

## Experiments / Findings

- 人类在 300 个问题上平均准确率 95.7%，而多个闭源和开源 MLLM 在简单 pattern 上显著失败，说明 benchmark 测到的是视觉 grounding 缺陷而非题目难度。
- CLIP pattern-level weakness 与显式使用 CLIP 的 LLaVA/InstructBLIP 强相关，相关系数均超过 0.7；因此视觉 encoder 的缺陷会直接传递到 MLLM，而扩大语言模型未必能修复。
- 放大 CLIP 分辨率的收益很小，放大网络规模也不能系统解决问题；加入 DINOv2 的 MoF 能补充 CLIP 的颜色/语义与 SSL 的视觉细节，但 feature 混合比例会影响指令跟随。

## Ablation / Error Analysis

消融交换选项顺序、问题措辞、CLIP backbone/resolution、DINOv2 additive/interleaved feature 和是否带解释。选项交换仍导致不稳定，证明错误不是简单的 label bias；MoF 中某一 encoder 的优势可能被另一 encoder 的信息冲突抵消，且视觉 grounding 不佳时语言模型会生成貌似合理的解释。

## Limitations

MMVP 是小规模、二选一、刻意设计的诊断 benchmark，不能代表开放式视觉推理或真实图片分布。相关性分析不能证明 CLIP 是唯一根因，MoF 也只是补丁；开放模型、闭源模型和不同 prompt 的接口差异会影响可比性。

## 两句话总结

论文用 MMVP 揭示 MLLM 在极简单视觉细节上仍存在巨大盲点，并证明 CLIP 视觉 encoder 的 pattern-level 缺陷会被语言模型继承。DINOv2/CLIP feature 混合提供了修复方向，但更大语言模型和更高分辨率本身并不能保证真实视觉理解。
