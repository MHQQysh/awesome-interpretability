# 209. Probing Conceptual Understanding of Large Visual-Language Models

- **Authors:** Madeline Schiappa, Raiyaan Abdullah, Shehreen Azad, Jared Claypoole, Michael Cogswell, Ajay Divakaran, Yogesh Rawat
- **Venue:** CVPR Workshop 2023
- **Paper:** https://doi.org/10.1109/CVPRW63382.2024.00186
- **Tags:** VLM Probing, Conceptual Understanding, Relations, Composition, Context

## Introduction

VLM 在检索、分类和 VQA 上的高分不一定代表形成了人类式 conceptual map；它可能只记住对象和语言共现。论文用认知科学启发的 probe 检查模型能否理解关系、组合和上下文，例如“雪地里出现不相称的物体”或“物体放到不合理背景”是否改变判断。

## Method / Framework

构建三个数据集：Probe-R 测 subject-object/predicate relations，Probe-C 测 composition-object 组合，Probe-B 测 background-object contextual expectation。通过 predicate/object/background swap 形成 image-text matching 对，比较原图与违反常识的替换；还提出 selective positive/negative pairing 和 ITM+contrastive loss 微调 CLIP，奖励关系与组合保持。

## Baselines / Comparisons

评估 CLIP、ViLT、BridgeTower、FLAVA 等近期 V+L 模型，并和 VL-CheckList、ARO、SVLC、CREPE、SugarCREPE 等已有 relation/composition probe 对照。指标是各 Probe 数据集的匹配准确率/置信度变化，另外观察 cross-attention、CNN 与 ViT backbone 的差异。

## Experiments / Findings

- 多数现有模型在 compositional、relational、contextual probe 上失败，特别是把对象放入违反预期背景后性能几乎不变，说明模型未使用上下文构建概念地图。
- cross-attention 或并行 modality-specific attention 能改善 relation understanding；CNN 更擅长 texture/pattern，ViT 更擅长 color/shape，架构归纳偏置决定了不同概念能力。
- selective negative training 在 RelComp 等任务上提升 relation/composition，代价是标准分类性能有小幅下降；这表明“更懂概念”和“更会做常规匹配”并不完全一致。

## Ablation / Error Analysis

消融 predicate swap、composition swap、background swap、cross-attention 结构与 selective negative sampling。错误常来自语言模板过强、对象识别失败、背景只被当作风格而非语境，或模型用文本先验覆盖视觉冲突；因此单一 image-text score 不能区分真正概念理解与表面匹配。

## Limitations

probe 是行为测试，不能直接揭示模型内部哪一层表示了概念；合成 swap 的自然度和文本模板也会影响结果。数据规模和概念类型有限，不能代表开放世界因果、时间、数量或多步视觉推理，微调收益也不意味着通用 conceptual map 已形成。

## 两句话总结

论文用关系、组合、上下文三类认知启发 probe 证明许多 VLM 的高下游分数仍掩盖了薄弱的 conceptual understanding。选择性负样本能改善关系/组合能力，但背景常识、架构偏置和行为测试到内部机制的距离仍是核心问题。
