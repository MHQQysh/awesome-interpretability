# 154. DEPN: Detecting and Editing Privacy Neurons in Pretrained Language Models

- **Authors:** Xinwei Wu, Junzhuo Li, Minghui Xu, Weilong Dong, Shuangzhi Wu, Chao Bian, Deyi Xiong
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.174
- **Tags:** Privacy, Neurons, Model Editing, Memorization

## Introduction

预训练 LM 会记忆并复述训练语料中的姓名、ID、电话等个人信息，单纯过滤关键词不能修复内部记忆，训练阶段的去重或隐私保护又无法应对训练完成后才发现的泄漏。作者假设私密信息与知识一样集中在一组可定位 neuron 中，因此可以做 neuron-level detection 和 editing。

## Method / Framework

DEPN 的 privacy neuron detector 用 integrated gradients 计算多个 privacy markers 对 neuron activation 的归因，把 token / marker 级贡献聚合为 privacy attribution score。选取 top-z privacy neurons 后，把它们的 activation 设为零，实现局部编辑而不更新全模型。

批处理场景加入 privacy neuron aggregator，将多个实体或标记对应的 neuron 集合合并，减少逐条编辑的成本。作者从 model size、training time、prompt 改写和 privacy neuron distribution 多角度验证检测稳定性。

## Baselines / Comparisons

与数据预处理去隐私、训练阶段 memorization reduction、post-hoc fine-tuning / model unlearning 和 keyword blocking 比较。指标包括 private information exposure / extraction success、非隐私语言建模性能、编辑后的 general QA，以及不同 z、marker 和模型规模的稳定性。

## Experiments / Findings

- 把 top privacy neurons 置零能显著降低个人信息暴露，同时在普通任务和语言建模性能上保持较小损失。
- privacy attribution 并非随机分布；不同隐私 marker 会共享部分 neuron，也会在层间形成稳定分布，支持“privacy information 有局部神经承载”的假设。
- 模型越大、训练越久，memorization 与可检测 privacy neuron 的关系更明显；prompt 变体仍能找出相关 neuron，说明方法不只记住一个固定 query。
- 批量 aggregator 可以一次编辑多个私人实体，比逐实体参数更新更便宜，适合在模型发布后处理一批删除请求。

## Ablation / Error Analysis

主要消融 neuron 数量 z、integrated gradients 与其它 attribution、单条 marker 与多 marker aggregator、activation zeroing 与完整重训练。z 太小会漏掉泄漏路径，太大则损害正常知识；只屏蔽输出关键词不能防止 paraphrase 或多步 prompt 重新诱导信息。部分 privacy neuron 与通用语义共享，因此编辑可能带来 collateral damage。

## Limitations

隐私定义聚焦个人身份信息，不能覆盖所有敏感属性。zeroing 是启发式，不保证模型真正忘记数据，攻击者仍可能从其它 neuron、tokenization 或外部知识恢复。实验多在可访问的预训练模型上进行，闭源 API 无法执行 neuron editing；隐私评估也不能证明符合具体法律删除义务。

## 两句话总结

DEPN 把隐私泄漏定位为可检测、可编辑的 neuron 集合，用 integrated gradients 找出 privacy neurons，再通过 activation zeroing 降低记忆复述。结果支持局部神经编辑比关键词屏蔽更接近内部修复，但共享表示、残余泄漏和黑盒不可访问仍是关键问题。
