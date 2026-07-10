# 330. Towards Explainable Harmful Meme Detection through Multimodal Debate between Large Language Models

- **Authors:** Hongzhan Lin, Ziyang Luo, Wei Gao, Jing Ma, Bo Wang, Ruichao Yang
- **Venue / year:** WWW 2024
- **Paper:** https://arxiv.org/abs/2401.13298
- **Tags:** Harmful Meme Detection, Multimodal Debate, Explainability, LLM Judge

## Introduction

有害 meme 的隐含意义来自图像、文字、文化常识和二者冲突，普通 multimodal classifier 只能输出 harmful/harmless，无法说明为何。ExplainHM 的问题是能否让 LLM 从相反立场生成可读 rationale，再用小模型将 rationale 与 meme 的视觉文本信息融合。

## Method / Framework

第一阶段让 multimodal LLM debaters 分别从 harmless 和 harmful 立场解释 meme，形成 contradictory rationales；第二阶段 fine-tune 一个 small language model 作为 debate judge，读取两个立场的理由与 meme intrinsic multimodal representation，进行 harmfulness inference。辩论机制用 divergent/CoT reasoning 暴露隐含文化、讽刺和语义冲突，同时保留小模型部署成本。

## Baselines / Comparisons

实验在三个 public harmful meme datasets 上比较传统 unimodal BERT/Faster R-CNN、two-stream multimodal models、pretrained VLM fine-tuning、global/local interaction、prompt/caption methods 和不使用 debate/rationale 的 small judge。指标包括 Accuracy、Macro-F1 以及 rationale quality/可读性。

## Experiments / Findings

ExplainHM 在三个数据集上优于已有 harmful meme detection baselines，并能输出两方理由帮助用户理解隐含伤害；WWW PDF 示例显示宗教、疫苗和文化讽刺 meme 需要 commonsense/context 才能解释。公开表格中的一个 rationale-order 对照显示 Harm-C/Harm-P/FHM accuracy 约 85.59/88.44/71.40 与反向顺序 85.03/90.17/71.00，说明输入顺序会影响 judge。

## Ablation / Error Analysis

去掉 harmful/harmless debate、只用单一 rationale、改变 rationale order、替换 small judge/backbone 是关键消融；Harm-P/FHM 等数据集的文化隐含意义更难，LLM rationale 可能带入偏见或过度解释。debater 产生的理由不是 ground-truth，judge 也可能被更长/更有说服力的一方影响。

## Limitations

有害性是社会和文化依赖的，自动 rationale 可能传播敏感内容或错误指控；三个数据集不能覆盖所有语言/文化。LLM debate 推理成本高，small judge 的解释忠实性仍需人工和反事实验证。

## 两句话总结

ExplainHM 让多模态 LLM 从 harmful/harmless 两个立场辩论生成理由，再由小模型结合 meme 图文信息判断有害性，实现检测与解释一体化。它在三个数据集上优于传统方法，但 rationale bias、文化依赖、顺序敏感和推理成本限制实际部署。

## Evidence note

本笔记核对 WWW 2024/arXiv PDF 的 debate framework、datasets、baseline taxonomy、rationale-order Table 8 与 case studies。

