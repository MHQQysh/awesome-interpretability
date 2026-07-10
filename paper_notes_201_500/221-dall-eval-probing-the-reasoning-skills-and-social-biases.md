# 221. DALL-EVAL: Probing the Reasoning Skills and Social Biases of Text-to-Image Generation Models

- **Authors:** Jaemin Cho, Abhay Zala, Mohit Bansal
- **Venue:** ICCV 2023
- **Paper:** https://doi.org/10.1109/ICCV51070.2023.00283
- **Tags:** Text-to-Image Evaluation, Visual Reasoning, Social Bias, DALL-E

## Introduction

Text-to-image models 能生成高保真图像，但“看起来像”不等于理解对象数量、空间关系或社会属性。论文构建统一评测，分别测 visual reasoning skills 与 social bias，避免只用 CLIP score 或人类偏好评价生成质量。

## Method / Framework

PAINTSKILLS 是 compositional diagnostic dataset，测试 object recognition、object counting、spatial relation understanding；提示把多个对象/关系组合进文本，再由人工或视觉问答程序检查生成图像是否满足约束。社会偏差实验用 gender、skin tone 和职业/服饰等 prompt 配对，统计生成属性分布并用 MAD 衡量偏离均匀分布的程度。

## Baselines / Comparisons

评估 DALL-E 系列、Karlo、Stable Diffusion 等文本到图像模型，比较不同 prompt、模型版本和生成结果。指标包括 reasoning skill accuracy、object/attribute presence、gender/skin-tone distribution 与 MAD，而不是单一图文相似度。

## Experiments / Findings

- 高保真模型在对象识别之外仍会错数量和空间关系，说明生成器可能利用局部语义共现而未形成稳定 compositional scene representation。
- 社会偏差不是单纯“生成随机人物”：gender 与衣物/职业属性存在系统相关，模型间 bias 程度不同；表中 gender MAD 约 0.20--0.36，skin-tone MAD 约 0.17，明显高于理想 uniform=0。
- 生成质量、reasoning 与 fairness 彼此不等价，扩展模型规模或逼真度不能自动消除错误和偏差；诊断 prompt 能暴露模型内部先验的可见后果。

## Ablation / Error Analysis

消融对象数量、关系类型、prompt 模板、性别/肤色配对和生成采样。复杂组合更易出现漏对象、错方向和错误绑定；bias 指标会受到检测器识别误差、prompt 词义和生成随机性影响，不能把单次图像当作模型总体偏见。

## Limitations

诊断集合覆盖的对象与社会属性有限，视觉检测器/人工标注仍有偏差；MAD 只描述分布不均，不等于完整公平性。文本到图像模型的版本、采样器和提示策略会改变结果，评测也不能直接揭示生成网络的因果机制。

## 两句话总结

DALL-EVAL 用 compositional PAINTSKILLS 和社会属性分布评测证明，文本到图像模型的逼真度并不代表对象关系推理或公平性。它把“会画图”拆成可测的识别、计数、空间和 bias 维度，但诊断覆盖与偏差归因仍需扩展。
