# 091. CIKT: A Collaborative and Iterative Knowledge Tracing Framework with Large Language Models

> 人工精读笔记：EMNLP 2025。重点核对了 Analyst/ Predictor 分工、九个基线、profile 消融和迭代敏感性。

## 0. 论文信息

- **作者**：Runze Li, Siyu Wu, Jun Wang, Wei Zhang
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.975)
- **模型**：Llama3.1-8B-Instruct、Qwen2.5-7B-Instruct
- **数据**：ASSIST2009、ASSIST2012、Eedi、BePKT；指标为 ACC、F1 和长序列 ACC

## 1. Introduction：要解决什么问题

Knowledge Tracing 需要根据学生历史答题记录估计知识状态，并预测下一题是否答对。BKT 等早期方法参数可解释但难以表示复杂时序依赖，DKT/SAKT/AKT 等深度模型准确率更高却通常是黑盒；直接用通用 LLM 又难以产生结构化学生画像，也缺少根据领域反馈持续改进的机制。

CIKT 的问题定义是同时获得预测性能和可读解释：一个模块负责把交互历史转成动态 student profile，另一个模块使用 profile 做下一题预测，并且让预测效果反过来改进画像生成器，而不是只在部署后静态调用 LLM。

## 2. Method / Framework：怎么解决

### 2.1 Analyst

Analyst 读取按时间排序的答题、知识点、题目难度和正确性，生成包含掌握情况、误解、薄弱概念和需要关注内容的自然语言 profile。初始 profile 可以由 GPT-4o distillation warm-start，再由主模型生成。

### 2.2 Predictor

Predictor 以 profile 加历史交互作为输入，输出下一题正确/错误概率。训练目标是二元交叉熵；profile 不是事后解释，而是推理时真正进入 Predictor，因此可以检查它是否具有预测效用。

### 2.3 Collaborative iteration

先生成 profile，再训练 Predictor；用 Predictor 在正确性上的反馈指导 Analyst 更新，随后用新 profile 重新训练 Predictor，形成 Analyst-Predictor 互相增强的循环。作者用 KTO 风格的 preference/reinforcement objective 让正确预测对应的 profile 更受偏好。

## 3. Baseline / 对比基线

- **BKT 系列背景**：可解释但表达能力有限。
- **DLKT**：DKT、DKVMN、SAKT、AKT、LPKT、FoLiBi 等代表 RNN、memory、attention、forgetting dynamics 路线。
- **更近期 KT**：IKT、DIMKT 等具备认知或解释建模的基线。
- **通用 LLM**：GPT-4o、DeepSeek-R1 直接做 KT，检验专门结构是否必要。
- **CIKT 变体**：去掉 iteration、去掉 Analyst-Predictor cooperation、推理时去 profile、去 GPT-4o distillation，分别隔离训练流程和 profile 的作用。

## 4. Experiments / Findings：结果如何读

在 ASSIST2009、ASSIST2012、Eedi 和 BePKT 上，CIKT-Qwen2.5-7B 和 CIKT-Llama3.1-8B 总体超过最强传统 KT baseline，尤其在交互长度大于 15 的长序列上优势更明显。表 1 中 Qwen 版本在多个数据集的 ACC/F1 达到约 0.777--0.852，且直接 GPT-4o/DeepSeek-R1 明显更弱，说明“有 LLM”不等于“适合 KT”。

### 4.1 长序列与可解释性

动态 profile 将长历史压缩成可阅读的掌握状态，既减少 Predictor 直接处理长序列的困难，又保留概念级信息。案例分析显示，迭代后的 profile 能更清楚地区分学生已掌握内容、反复错误和随时间变化的薄弱点。

### 4.2 消融

去掉 iteration 会出现清晰的性能下降，说明收益不只是 profile 格式；同时去掉 iteration 和 cooperation、退化为直接 LLM fine-tuning 时下降更大。训练完整但推理时 withholding profile 的变体退化最明显，证明 profile 是预测链条中的有效输入，而不是装饰性 explanation。

去掉 GPT-4o distillation 只带来边际下降，说明外部模型主要负责 warm-start，CIKT 的核心收益来自协同迭代。迭代轮数从 0 到 3 时性能通常在约 3 轮稳定，iteration sample size 增大有收益但在高端出现边际递减。

## 5. Ablation / 机制解释

作者还用 profile quality agreement matrix 和定性案例检查画像是否可读。这里要区分两种证据：profile 被人看懂并不自动证明预测正确，而 profile 被推理时移除后性能下降，才说明它对模型决策有功能作用。CIKT 的机制链是“历史交互 -> 画像 -> 预测 -> 反馈改进画像”。

## 6. Limitations / 局限与复现注意

- 画像质量仍依赖 LLM 生成和 prompt；人工过滤只做格式/明显矛盾清理，不是完整教育专家标注。
- 迭代过程需要多次 LLM 训练/推理，外部成本和延迟可能高于普通 KT。
- 四个数据集主要是离线二元答题预测，真实课堂中的知识迁移、课程顺序和教师干预尚未验证。
- 自然语言 profile 可能看起来合理但包含错误归因，需要独立教育专家和长期干预实验评估。

## 7. 两句话总结

CIKT 解决传统 KT 黑盒、通用 LLM 缺少结构化学生状态以及无法持续自改进的问题。它用 Analyst 生成动态画像、Predictor 使用画像预测，并用 KTO 风格迭代互相优化，在四个数据集上优于传统 KT 和直接 LLM，消融表明 iteration、cooperation 和推理时 profile 都是关键。
