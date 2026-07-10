# 114. Towards Interpretable Sequence Continuation: Analyzing Shared Circuits in Large Language Models

> 人工精读笔记：EMNLP 2024。重点是找出 number words、months、colors 等 sequence continuation 任务共享的 attention/MLP circuit。

## 0. 论文信息

- **作者**：论文作者团队
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.699)
- **模型/任务**：GPT-2、Llama-2-7B；数字、月份、颜色和其他序列续写

## 1. Introduction：要解决什么问题

LLM 能完成很多看似相似的下一个元素预测，但不同 sequence continuation task 是否复用同一 circuit，还是每个 task 都有独立模块，尚不清楚。共享 circuit 对解释模型抽象能力、迁移和安全干预很重要。

## 2. Method / Framework：怎么解决

作者用 iterative node/edge ablation 建立每个任务的 circuit，以 logit difference 衡量正确 token 相对错误 token 的贡献。随后计算不同任务重要组件集合的 intersection，并做 targeted ablation 和 random ablation 对照。

还分析 attention pattern、successor heads、MLP vocabulary output 和 logit lens，尝试把共享组件映射到“顺序关系”“复制/路由”和最终 token 生成。

## 3. Baseline / 对比基线

- 逐任务独立 circuit。
- 随机选择同数量 attention/MLP 组件。
- 不 ablate 的完整模型。
- GPT-2 vs Llama-2，检查共享机制是否跨规模。
- 单个组件 vs 组件集合，区分冗余和协同。

## 4. Experiments / Findings：结果如何读

不同 sequence continuation task 存在共享 circuit subgraphs，但共享的是更抽象的“顺序延续/下一项”功能，并不意味着所有 token-specific mechanism 相同。某些 attention heads 检测序列成员关系，再把信息传给后续 head/MLP；相应 MLP 负责将 next element 写入 vocabulary。

对组件集合做 ablation 会显著破坏正确续写，随机 ablation 只影响约 3--28% 的样本；对 GPT-2 某些重要集合，破坏率可达 92--100%，说明发现的 circuit 不是任意相关 head。数学/intervaled prompt 的结果较不稳定，表明共享抽象存在边界。

## 5. Ablation / 机制解释

sequence type、模型、prompt template、组件集合与随机集合、attention/MLP 分别消融；实验还测试 ablation 后模型是否能由 backup components 补偿。重要 head 常表现为 context pattern detection，MLP 以 token-specific vocabulary promotion 完成续写。

## 6. Limitations / 局限与复现注意

任务数量和 prompt 样本有限，序列 token 多为单 token；circuit discovery 的阈值和迭代顺序可能影响集合；shared circuit 不等于完整算法，未覆盖自然语言长程续写和真实推理。

## 7. 两句话总结

论文发现多个序列续写任务会复用部分 attention/MLP circuit，尤其是识别序列关系和写入下一 token 的组件。针对性 ablation 比随机 ablation 更能破坏正确续写，支持共享抽象的存在，但任务简单、样本有限且 circuit 发现依赖阈值。
