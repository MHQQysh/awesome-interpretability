# 112. Advancing Large Language Model Attribution through Self-Improving

> 人工精读笔记：EMNLP 2024。重点是用 reverse attribution 自造数据，再迭代 relabel/preference optimization 提升 grounded citation。

## 0. 论文信息

- **作者**：Lei Huang, Xiaocheng Feng, Weitao Ma, Liang Zhao, Yuchun Fan 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.223)
- **方法**：START，Self-Taught AttRibuTion

## 1. Introduction：要解决什么问题

LLM attribution 需要回答不仅正确，还要引用支持对应 claim 的文档；高质量 attribution data 昂贵且需要人工。直接让较小模型生成 citation 常有格式正确但证据不相关的问题，已有 self-improvement 在数学 reasoning 中有效，却未必适用于 attribution。

## 2. Method / Framework：怎么解决

START 先用 synthetic warm-up：模型生成 answer，再把答案拆成 claims，并把每个 claim 反向链接到支持它的 source document，形成细粒度 attribution 数据。随后模型用自己的输出进行 relabel、比较多种 attribution、筛选高质量偏好对，并迭代 preference optimization。

该流程将“回答生成”和“证据选择”绑定，reward 不只看 citation format，还看 claim-document relevance、coverage 和 groundedness。

## 3. Baseline / 对比基线

- 直接 SFT/teacher distillation 的 attribution model。
- GPT-4 生成或标注的 attribution data。
- 不迭代的 synthetic warm-up。
- 不做 relabel/preference optimization 的单轮训练。
- 不同 source aggregation 与 citation format。

## 4. Experiments / Findings：结果如何读

在三个 open-domain QA benchmark 上，START 随迭代轮次提升 answer attribution quality，并在多文档聚合时减少不支持 citation。论文报告自生成的高质量 preference signal 可以补偿人工标注不足，说明 attribution 不是只能靠昂贵人工数据训练。

分析表明，synthetic warm-up 主要解决“模型不会按格式引用”，self-improving/relabel 则进一步解决“引用看似存在但没有支撑 claim”。跨模型和数据集的收益不完全相同，source 数量、回答长度和文档噪声会影响迭代收益。

## 5. Ablation / 机制解释

去掉 warm-up、去掉 attribution relabel、去掉 preference optimization、减少迭代轮次分别降低效果；比较单文档与多文档场景，说明 self-improvement 对证据组合尤为重要。

## 6. Limitations / 局限与复现注意

模型自造 attribution data 可能把错误复制到下一轮，必须用过滤器或外部 verifier；自动 reward 不能完全保证事实真值；长答案和多跳证据的 citation granularity 仍难处理。

## 7. 两句话总结

START 通过 reverse attribution 自建 claim-document 数据，再用 relabel 和 preference optimization 迭代提升 LLM 的 grounded citation 能力。实验支持“先学会格式、再自我纠错证据选择”的流程，但自训练错误累积和自动 reward 偏差仍是核心风险。
