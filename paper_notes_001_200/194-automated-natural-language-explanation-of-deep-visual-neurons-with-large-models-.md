# 194. Automated Natural Language Explanation of Deep Visual Neurons with Large Models (Student Abstract)

- **Authors:** Chenxu Zhao, Wei Qian, Yucheng Shi, Mengdi Huai, Ninghao Liu
- **Venue:** AAAI 2024 Student Abstract
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/30537
- **Tags:** Visual Neuron, Natural Language Explanation, LLM, VLM

## Introduction

视觉神经元可视化能找到激活区域，却通常还需要人工给这些区域命名。随着模型规模和神经元数量增长，人工逐个检查不可扩展；论文研究如何让大模型从神经元最强激活的 image patches 自动生成自然语言语义，并尽量避免依赖人工先验。

## Method / Framework

流程分三步：先收集目标 classification neuron 激活最高的 image patches；再用 LLM 根据这些 patch 构造领域相关的 explanation vocabulary；最后用视觉语言模型把候选词/短语与 patch 的视觉特征做相似度匹配，生成目标神经元的自然语言解释。这样既利用 LLM 的语言概括能力，又用 VLM 检查解释是否和真实视觉区域对应。

## Baselines / Comparisons

论文是 student abstract，主要与传统 neuron visualization 和需要人工命名/解释的流程对比，并从 qualitative examples 与 quantitative analysis 两方面验证。比较重点不是分类 accuracy，而是解释能否识别如“拱形结构、跨越水面”等语义、是否与高激活 patch 一致，以及自动流程是否减少人工 intervention。

## Experiments / Findings

- 激活 patch 为解释提供了输入证据，LLM 生成的候选概念能把低层视觉响应转为短语级语义，而不是只返回一张热力图。
- VLM 相似度步骤用于在多个候选描述中保留与图像区域更匹配的概念，定性案例显示解释可以覆盖物体、形状和场景关系等语义。
- 论文报告 qualitative 与 quantitative 结果都支持自动化框架的有效性，但它的贡献更接近可扩展的解释管线原型，而不是大规模统一 benchmark 上的 SOTA 宣称。

## Ablation / Error Analysis

核心组件是 patch collection、LLM vocabulary construction 和 VLM matching；去掉 patch 证据会让解释退化成语言模型的先验猜测，去掉 VLM 对齐则更容易出现“语言听起来合理但图像不支持”的描述。神经元可能响应多个相关概念，激活样本偏差、候选词粒度和视觉-文本模型偏差都会造成解释过宽或过窄。

## Limitations

这是 student abstract，方法细节、数据规模、基线实现和误差统计都比完整论文简略。后验自然语言解释不一定等于神经元的唯一功能，LLM/VLM 也可能把共现物体误当作神经元真正检测的特征；对生成式视觉语言模型和跨层神经元的适用性尚未展开。

## 两句话总结

论文用高激活 image patches、LLM 候选概念和 VLM 相似度，把深度视觉神经元的人工命名转成可扩展的自然语言解释流程。它证明了“语言概括 + 视觉对齐”比单纯可视化更有语义，但作为 student abstract 仍需要更完整的量化 benchmark 与因果干预。
