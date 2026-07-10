# 104. Unveiling Factual Recall Behaviors of Large Language Models through Knowledge Neurons

> 人工精读笔记：EMNLP 2024。重点是 Two-hop Factual Reasoning for Knowledge Neurons、CoT 对 factual recall 的影响和 knowledge distraction。

## 0. 论文信息

- **作者**：Zeping Yu, Sophia Ananiadou 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.420)
- **对象**：knowledge neurons；单跳与两跳事实、CoT、知识干扰

## 1. Introduction：要解决什么问题

LLM 能记住大量事实，但多跳 reasoning 时可能忘记第一跳结果、错误使用 parametric knowledge，或在 CoT 中把事实召回和推理混在一起。已有 knowledge neuron 主要在单跳模板中定位 factual memory，尚不清楚这些 neurons 在每一步 reasoning 中如何被调用。

论文构造 TFRKN（Two-hop Factual Reasoning for Knowledge Neurons），观察第一跳和第二跳的 knowledge neurons 是否被正确激活，并测试增强/抑制这些 neurons 是否改变事实召回和最终 reasoning accuracy。

## 2. Method / Framework：怎么解决

对 relation query 找到 active knowledge neurons，定义 knowledge neuron score，并将两跳问题拆成 s-r1-o1、o1-r2-o2。实验条件包括 no CoT、zero-shot CoT、直接单跳 baseline，以及分别增强/抑制第一跳、第二跳或两者 neurons。

作者还构造 knowledge distraction：在问题中引入与 subject/中间实体相关但不正确的知识，观察模型是否被强关联 shortcut 带偏。增强实验对照 Base、Enhance、Suppress 和 Random，随机干预用于控制只是改 activation 带来的影响。

## 3. Baseline / 对比基线

- **Single-hop factual recall**：每一跳单独回答，确认知识本身是否存在。
- **No-CoT / zero-shot CoT**：比较显式 reasoning 对 recall 的影响。
- **Base**：不干预 knowledge neurons。
- **Enhance/Suppress**：目标 hop 的 KN activation 增强或抑制。
- **Random intervention**：同样规模随机 neuron 控制。
- **First-hop/second-hop/both**：定位哪一步对两跳失败更敏感。

## 4. Experiments / Findings：结果如何读

论文发现两跳 reasoning 中，超过三分之一的案例存在 factual recall 问题：第一跳或第二跳的 knowledge neuron 没有按正常 trajectory 工作。CoT 通常加强被动的内部 fact recall，使模型更能保留第一跳结果，但 CoT 也可能引入 shortcut 和额外干扰。

增强正确 hop 的 KN 通常提升 recall/reasoning，抑制相关 KN 会显著损害对应事实；抑制第一跳会阻断第二跳输入，抑制第二跳则直接影响最终 object。随机干预的影响较小，支持 KN attribution 不只是 activation correlation。

知识 distraction 实验显示，输入中引入与中间实体强相关的错误信息会让模型把错误 subject/attribute 作为检索线索，导致 factual recall 受阻。这个 finding 解释了为什么多跳任务即使两条单跳事实都能回答，也可能在组合时失败。

## 5. Ablation / 机制解释

- no-CoT vs zero-shot-CoT，分离显式 reasoning 对 factual recall 的影响。
- first-hop/second-hop/both suppression，定位错误传播位置。
- Enhance/Suppress/Random，建立 KN 的功能因果证据。
- knowledge distraction，测试模型是否依赖错误但高关联的 shortcut。

## 6. Limitations / 局限与复现注意

- 两跳事实与模板化 query 简化了真实开放推理。
- KN score 与 neuron 集合依赖 relation、prompt 和阈值；不同 template 可能找到不同集合。
- CoT 影响同时包括语言提示与内部计算，不能只归因于 neurons。
- 知识 distraction 的构造不覆盖所有现实世界冲突和检索设置。

## 7. 两句话总结

论文用 TFRKN 追踪两跳事实召回，发现 CoT 会加强部分被动 recall，但中间实体和关系的知识 neurons 仍可能被遗忘或被高关联错误知识干扰。增强/抑制对应 hop 的 KN 能改变最终结果，说明它们具有功能作用，但模板化两跳任务限制了外推。
