# 373. MoE-LLaVA: Mixture of Experts for Large Vision-Language Models

- **Authors:** Bin Lin, Zhenyu Tang, Ye Yang, Huang Jinfa, Junwu Zhang, Yatian Pang, Peng Jin, Munan Ning, Li Yuan
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2401.15947
- **Tags:** `LVLM` `Mixture-of-Experts` `sparsity` `hallucination` `routing`

## Introduction
扩大 LVLM 的参数量和多模态训练数据通常能改善视觉理解，但 dense 模型每个 token 都激活全部参数，训练和部署成本快速上升。MoE 可以让不同 token 只经过少数 experts，却有一个关键问题：如果在把 LLM 转成 LVLM 的同时直接稀疏化，模型容易严重掉点，且路由行为难以解释。

本文的核心问题不是提出一个解释器，而是把“哪个 expert 处理哪个 token、稀疏化为什么没有直接破坏能力”变成可观测的 routing 结构。论文还把 object hallucination benchmark POPE 作为主要诊断场景，观察稀疏模型的视觉 grounding 是否保持。

## Method / Framework
MoE-LLaVA 使用 MoE-Tuning 三阶段训练：

1. **Stage I:** 冻结 LLM 和视觉编码器，只训练 MLP projector，让视觉 token 进入语言模型的表示空间。
2. **Stage II:** 除 Vision Encoder 外训练整个 LLM/多模态部分，先获得一般的视觉-语言理解能力。
3. **Stage III:** 将已训练的 FFN 权重复制为多个 expert，只训练 MoE 层和 router。每个 token 由 router 选择 top-2 experts，其余 experts 不参与该 token 的计算。

作者比较不同 expert 数量、top-k、MoE 插入位置以及仅训练 FFN/训练全部参数的策略。路由可视化按 layer、token modality 和 benchmark 展示 top-1/top-2 激活路径，用于观察是否出现“某 expert 专门处理图像”这样的解释。

## Baselines / Comparisons
主要比较对象包括 LLaVA-1.5-7B/13B、BLIP-2、MiniGPT-4、mPLUG-Owl2、LLaMA-VID、InternVL-Chat、MobileVLM、Chat-UniVi 等 dense 或不同规模 LVLM。评价覆盖 ScienceQA、GQA、SQA-I、VQAv2/TextVQA、POPE、MMBench、LLaVAW 和 MM-Vet；POPE 进一步包含 Random、Popular、Adversarial 子集，用来检查对象幻觉而不只是总体回答分数。

论文还用相同基础模型内部的配置做 controlled baseline：1 expert vs 2 experts、top-1 vs top-2、FFN-only vs all-parameter training、不同 architecture interval，以及有没有 Stage III。这样比较能把 MoE 架构收益与训练初始化收益区分开。

## Experiments / Findings
将模型扩展到约 3.6B sparse activated parameters 后，相对 LLaVA-1.5-7B，ScienceQA、POPE、MMBench、LLaVAW、MM-Vet 分别提升 1.9、0.4、0.9、30.7、3.8 个百分点。约 2.2B activated 的 MoE-LLaVA 在 POPE 上还超过 LLaVA-1.5-13B 约 1.1%，并接近 activated 参数约大 8 倍的 InternVL-Chat-19B。

内部消融显示，FFN-only 训练比训练全部参数更省时且略好：表中 FFN 配置的 GQA/VisWiz/VQAT/POPE/LLaVAW 为 61.5/32.6/48.0/87.0/88.7，All 配置为 61.3/31.9/47.6/87.0/88.1。两 experts 优于一个 expert；top-2 相比 top-1 在 VQAv2、GQA、SQA-I、VQAT、POPE、LLaVAW 上达到 76.2/61.5/63.1/48.0/88.7/，明显优于 top-1 的 74.5/58.4/58.0/44.0/85.7/。

路由图的主要 finding 是：不同 expert 在浅层/深层的激活比例不同，但没有稳定的“图像 expert”和“文本 expert”硬分工；很多 expert 同时处理两种 modality。这意味着 routing 是一种条件计算机制，不应被简单解释成语义模块。

## Ablation / Error Analysis
架构间隔实验比较前半层、后半层、间隔插入和全部层；间隔插入在约 20 小时训练成本下取得最好的综合结果，而全部层训练约 32 小时且不一定更好。Stage III 的 expert 初始化尤其关键：直接稀疏化会产生性能退化，先用 dense LVLM 的 FFN 复制初始化，再只优化 MoE 层，能把原有能力迁移到稀疏路径。

失败分析主要集中在训练稳定性和 expert specialization。论文报告 FP16 训练容易不稳定，且 experts 不一定自然形成清晰的功能分工；因此路由热图能告诉我们 token 走了哪条路径，却不能直接告诉我们该路径实施了哪个可读语义功能。POPE 的改善说明视觉幻觉有所缓解，但仍不能证明每个回答都忠实依据图像。

## Limitations
模型仍依赖 LLaVA 风格的视觉编码和指令数据，MoE routing 的解释主要是统计可视化而非因果干预。不同 expert 的功能没有被系统地用 activation patching、expert ablation 或 counterfactual routing 验证。训练稳定性、显存占用、expert 负载均衡和更大模型上的 scaling 仍是工程限制，跨模型的公平比较也会受到视觉编码器和训练数据差异影响。

## 两句话总结
MoE-LLaVA 先训练一个 dense LVLM，再复制 FFN 初始化 experts，并以 top-2 routing 进行稀疏多模态计算，从而用较少 activated parameters 保持或提升 LVLM 能力和 POPE 表现。它给可解释性研究提供了可观测的路由轨迹，但实验显示 expert 并不天然对应清晰的人类语义模块，routing heatmap 需要因果干预才能升级为机制解释。

## Evidence note
已读取 arXiv 2401.15947 v5 本地 PDF 文本；关键数字来自论文摘要、主 benchmark 对比、表 5 的组件消融和 routing visualization 讨论。
