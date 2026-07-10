# 102. Lookback Lens: Detecting and Mitigating Contextual Hallucinations in Large Language Models Using Only Attention Maps

> 人工精读笔记：EMNLP 2024。重点是 contextual hallucination 与 parametric hallucination 的区分，以及 attention-only detector 的跨任务迁移。

## 0. 论文信息

- **作者**：Yung-Sung Chuang, Linlu Qiu, Cheng-Yu Hsieh, Ranjay Krishna, Yoon Kim, James Glass
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.84)
- **代码**：[Lookback-Lens](https://github.com/voidism/Lookback-Lens)
- **任务**：document QA 与 summarization；训练/评测包含 NQ、XSum 等

## 1. Introduction：要解决什么问题

即使正确 passage 已经放进 context，LLM 仍可能生成与 context 不一致的内容，这被称为 contextual hallucination。已有 hallucination detector 常用 hidden state、NLI 或文本 entailment；它们可能依赖模型、需要滑窗或在跨任务/跨模型时失效。

作者提出一个简单假设：如果模型在生成时更多 attend 到 context、较少 attend 到自己已经生成的 token，答案更可能 grounded。由此设计只读取 attention map 的 Lookback Lens，并进一步用 detector 引导 decoding。

## 2. Method / Framework：怎么解决

对每个生成时间步和每个 attention head，计算 context tokens 的 attention weight 与 newly generated tokens 的 attention weight 比值，称为 lookback ratio。把所有 layer/head 的 ratio 拼接，再用线性 classifier 预测一个输出 span 是否 hallucinated。

训练标签来自 LLaMA-2-7B-Chat 在 NQ/XSum 上生成的回答，并由 GPT-4o 核验；实验报告 GPT-4o 标注的一致率约 97%。span-level 预测使 detector 能在生成过程中对一段文本打分，而不是只给整篇答案一个标签。

### Guided decoding

每一步采样多个 candidate chunk，用 Lookback Lens 对 candidate 的 attention ratio 评分，选择更不易 hallucinate 的 chunk。这样不改模型参数，只在 decoding 时避开低 groundedness 的候选。

## 3. Baseline / 对比基线

- **Text NLI**：fine-tuned DeBERTa-v3 entailment classifier。
- **强 NLI model**：Vectara 等现成 entailment detector。
- **Hidden-state detector**：使用 LLM hidden states 的线性/MLP classifier。
- **Layer-activation baseline**：用选定层表示代替 lookback ratio。
- **Sliding-window detector**：对上下文窗口做局部判断，比较 span 聚合策略。

## 4. Experiments / Findings：结果如何读

Lookback Lens 在 QA 与 summarization 上与更复杂的 hidden-state 和 NLI detector 相当或更好。表 2 中，QA 训练/summary 测试和反向 transfer 都保持较高 accuracy/F1；例如 Lookback Lens 在 QA->Sum 的 accuracy/F1 约为 88.3/87.1，在 Sum->QA 约为 86.2/85.3，显示跨任务特征迁移。

它还能从训练于 7B LLaMA 的 detector 直接迁移到 13B 模型，不再训练；相比 hidden-state detector 跨模型性能下降较小。这个结果支持 ratio 是相对架构不变的 grounding signal，但不意味着所有模型 attention 都同样可解释。

Guided decoding 在 XSum 上将 contextual hallucination 降低约 9.6%，在其他任务也有约 3.2% 的下降。收益来自选择候选 chunk，而不是靠事后过滤；因此可以在生成时阻止一部分低 lookback 输出。

## 5. Ablation / 机制解释

- context/generated attention ratio vs hidden states：验证简洁 attention feature 的充分性。
- QA 与 summarization 交叉迁移：检验任务泛化。
- 7B -> 13B transfer：检验模型泛化。
- ratio span aggregation、candidate 数量和 threshold：控制解码收益与成本。

## 6. Limitations / 局限与复现注意

- 自动 GPT-4o 标注不是完美 ground truth，context entailment 仍需人工审计。
- attention weight 是 grounding proxy，不是 causal proof；模型可能低 attention 也能正确使用已写入表示。
- candidate sampling 增加生成成本，长文本任务的收益取决于候选数量和 chunk 长度。
- 主要验证 LLaMA-family，跨 tokenizer、架构和多语言任务仍需测试。

## 7. 两句话总结

Lookback Lens 用“注意力回看 context 相对回看已生成 token 的比例”检测 contextual hallucination，并发现一个线性 ratio classifier 可跨任务、跨 7B/13B 模型迁移。把它用于候选引导 decoding 后，XSum hallucination 下降约 9.6%，但 attention ratio 仍只是 grounding proxy，不等于严格因果解释。
