# 478. Automated Natural Language Explanation of Deep Visual Neurons with Large Models

- **Authors:** Chenxu Zhao, Wei Qian, Mengdi Huai, Yucheng Shi, Ninghao Liu
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2310.10708](https://arxiv.org/abs/2310.10708)
- **Tags:** `vision-neuron` `CLIP` `LLM-explanation` `neuron-ablation`

## Introduction
视觉 neuron 的 activation patch 通常需要人类观察 top images 并手写语义，模型规模越大越难扩展。论文提出一个无需人工标注/先验的 post-hoc pipeline，把 target neuron 的 activated patches、LLM 生成的 domain vocabulary 和 CLIP patch-concept matching 组合成自然语言 explanation。

## Method / Framework
先收集某 neuron 在 ImageNet/Places365 上最激活的 image patches；再让 GPT-3 根据应用域询问生成短语 vocabulary；CLIP 计算 patch 与每个 concept 的相似度，选择高分 concept 作为 neuron description。最后把 description 与 neuron activation 做 ablation 对照，验证语义 neuron 是否真的影响类别预测。

## Baselines / Comparisons
比较五名 human observers 对 top-activating patches 的描述，覆盖 AlexNet/ResNet/ViT、ImageNet/Places365 和不同 layer。实验还用单 neuron zeroing 的 category accuracy drop 与 qualitative agreement 对照，而不是只展示漂亮文字。

## Experiments / Findings
自动说明通常与人类描述一致，且有时更细，例如把 ViT neuron 描述成“带数字/刻度的 clock face”。浅层 neuron 偏 colors/edges/textures，深层偏 beak、clothing、scene 等抽象语义；说明可从 ImageNet object 迁移到 Places scene。单 neuron ablation 会让特定 category accuracy 显著下降，最大 category drop 约 84%，支持这些 neuron 与类别输出存在实际影响。

## Ablation / Error Analysis
不同 model/layer 的 concept vocabulary 和 CLIP matching 是主要敏感点；一个视觉 neuron 可能对应多个相近概念，文本说明会把视觉共现误写成单一语义。ablation 证明“影响某类预测”，但不证明自动生成的 phrase 是 neuron 的完整功能或唯一解释。

## Limitations
研究集中在分类 CNN/ViT 和英语 phrase；CLIP/LLM 的先验会把解释限制在 vocabulary 中，也可能引入语言模型幻觉。patch aggregation、top activation selection 和单 neuron zeroing 都无法覆盖 distributed/polysemantic circuit。

## 两句话总结
论文用 LLM 构造领域概念词表、CLIP 对齐 activated image patches，自动生成视觉 neuron 的自然语言解释，并用 ablation 验证其预测影响。它把视觉解释从人工标注扩展到多架构，但词表先验和 patch-level correlation 仍限制机制忠实性。

## Evidence note
已读取 vocabulary/patch/CLIP pipeline、human baseline、ImageNet/Places365 qualitative 结果及单 neuron category ablation。
