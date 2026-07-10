# 132. Automatic Evaluation of Attribution by Large Language Models

- **Authors:** Xiang Yue, Boshi Wang, Ziru Chen, Kai Zhang, Yu Su, Huan Sun
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.307
- **Tags:** Attribution, Factuality, Evaluation, LLM-as-a-judge

## Introduction

带引用的 LLM 会把回答与外部 reference 绑定，但“有引用”不等于“引用支持了回答”。作者引用生成式搜索的人工审计结果：New Bing 和 Perplexity 生成的陈述只有约 52% 能被对应引用完全支持；逐条人工核验又昂贵且慢。已有工作多做 binary entailment，或者把支持程度分成 full / partial / none，无法清晰区分“与证据矛盾”和“证据没有说到”。

论文要解决的是一个更细粒度的 attribution evaluation：给定 query、answer、reference，自动判断 answer 是 attributable、extrapolatory 还是 contradictory，并比较 prompt LLM 与小模型微调两种可扩展方案。

## Method / Framework

### 任务定义

- **Attributable:** reference 完全支持回答。
- **Extrapolatory:** reference 相关但信息不足，无法验证回答中的全部内容。
- **Contradictory:** 回答与 reference 明确冲突。

### AttrScore 两条实现路线

1. **Prompting LLM。** 给 ChatGPT 或 GPT-4 明确的三分类定义和 query-answer-reference 三元组，要求输出标签及解释；零样本和 few-shot 都测试。
2. **Fine-tuning smaller LMs。** 从 fact-checking、NLI、summarization 和 QA 数据重用或模拟训练数据，再微调 RoBERTa、FLAN-T5、GPT-2、LLaMA、Alpaca、Vicuna 等不同规模模型。QA 数据被改造成更接近真实 attribution 的样本，以减少只在 NLI 形式上训练的问题。

评测集一部分来自已有任务模拟数据，另一部分是从 New Bing 真实生成搜索交互中人工整理、覆盖 12 个领域的 AttrEval-GenSearch。指标使用三类各自 F1 和 micro-F1；micro-F1 在这里等价于总体 accuracy。

## Baselines / Comparisons

论文没有把一个 NLI 模型作为唯一 baseline，而是比较：zero-shot / few-shot prompting 的 GPT-4、ChatGPT、Alpaca；只用某一种相关任务数据微调；把 QA、NLI、fact-checking、summarization 组合起来微调；以及不同大小的 RoBERTa、FLAN-T5、GPT-2、LLaMA、Alpaca、Vicuna。这样能区分模型规模、提示格式和训练任务来源三种因素。

## Experiments / Findings

- GPT-4 在真实 New Bing 集上达到约 81% 到 83% 总体 accuracy，明显优于其它 prompting 模型，但作者明确认为距离生产环境可直接替代人工仍有差距。
- 小模型微调效果提升很大：Vicuna 13B 的 accuracy 在模拟集 / GenSearch 集从 zero-shot 的 34.6% / 41.4% 提升到 66.0% / 71.3%；FLAN-T5 770M 微调后甚至可以超过 ChatGPT。
- 真实集与模拟集的相对排名并不总一致，说明只在合成分布上优化会损害跨域泛化。
- 最难的是 contradictory。模型经常把回答中的数字、日期或因果关系与 reference 的冲突忽略掉，反而依赖自身参数知识，把它误判为 attributable。
- GPT-4 的错误分析中，30.6% 属于对细粒度信息不敏感，例如把 “131,930” 和 reference 中 “132,147” 当成可接受近似；22.2% 是误解标签定义和逻辑关系；13.9% 涉及等号、约等号等符号运算。

## Ablation / Error Analysis

任务来源消融显示，QA 和 fact-checking 数据对 attribution 最有帮助；组合多个相关任务总体更好，尤其改善 GenSearch 的 out-of-domain 测试。Prompt 消融比较 attribution、NLI、fact-checking、summarization 四种提示：fact-checking 和 NLI 提示通常优于专门 attribution 以外的简单提示，可能因为模型在指令微调阶段见过相似任务。一个重要的负结果是，增加示例不等于稳定提升，prompt 选择本身会改变零样本和 few-shot 的分数。

## Limitations

模拟训练数据和真实搜索错误的分布仍有差异，可能包含噪声、过于简单的错误模式和错误标签。小模型处理长 reference、数字比较和复杂逻辑的能力有限；GPT-4 虽然强，仍有成本、偏差和不可控性问题。三分类把每个 statement 配一个 reference，尚未覆盖多引用、多句答案和引用片段之间的跨句推理。

## 两句话总结

AttrScore 将引用核验从“是否被支持”细化为支持、证据不足和矛盾三类，并用真实生成式搜索数据检验 prompt judge 与小模型微调。结果表明 GPT-4 和任务迁移微调都有可用信号，但数字、逻辑和真正的 contradiction 仍是自动引用评估的主要瓶颈。
