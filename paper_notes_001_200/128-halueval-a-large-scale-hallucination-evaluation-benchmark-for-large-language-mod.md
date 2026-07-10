# 128. HaluEval: A Large-Scale Hallucination Evaluation Benchmark for Large Language Models

> 人工精读笔记：EMNLP 2023。重点是 generated-and-human-annotated hallucination benchmark，以及 detection 与 generation 两个方向。

## 0. 论文信息

- **作者**：Junyi Li, Xiaoxue Cheng, Wayne Xin Zhao, Jian-Yun Nie, Ji-Rong Wen
- **来源**：[EMNLP 2023](https://aclanthology.org/2023.emnlp-main.397)
- **资源**：HaluEval
- **场景**：general QA、knowledge QA、dialogue、summarization

## 1. Introduction：要解决什么问题

LLM hallucination 定义、类型和评测不统一；模型可能生成与 source 冲突、无法验证或语义上自洽但事实错误的内容。缺少大规模、多任务、同时含真实和 hallucinated samples 的 benchmark，导致 detection 方法难比较。

## 2. Method / Framework：怎么解决

HaluEval 用 sampling-then-filtering 自动生成候选 hallucinated answers，再由人工筛选/标注，覆盖 knowledge、dialogue、summarization 等任务。数据包含真实/错误回答、问题/上下文和 hallucination labels，支持训练 detector 与 zero-shot evaluation。

## 3. Baseline / 对比基线

zero-shot LLM judgment、fine-tuned classifier、prompt-based detection、不同 LLM 生成器和人工标注。指标包括 accuracy/F1，分别检查识别 hallucination 与生成 hallucinated answer 的倾向。

## 4. Experiments / Findings：结果如何读

HaluEval 显示强 LLM 并不总能可靠判断 hallucination，尤其是错误回答语言流畅、与常见知识一致时。不同任务/类型的 detection 难度差异大，模型会对表面线索、长度和 answer style 产生偏差。

自动生成加人工过滤提高了规模与质量平衡，但 benchmark 仍可能与生成器/过滤器分布相关。HaluEval 的价值是提供跨任务可复用的统一测试，而不是声称一个单一 accuracy 就能覆盖 factuality。

## 5. Ablation / 机制解释

sampling vs filtering、任务类型、生成模型、人工校验比例和 detector backbone 作为主要对照；不同 hallucination category 的结果用于分析错误来源。

## 6. Limitations / 局限与复现注意

自动生成样本可能带有模型偏差；人工标注成本与主观性仍存在；开放世界事实需要外部知识验证；benchmark 训练/测试污染会高估性能。

## 7. 两句话总结

HaluEval 通过 sampling-then-filtering 构建大规模、多任务 hallucination detection benchmark，覆盖 QA、dialogue 和 summarization。它显示流畅错误答案很难被模型可靠识别，并为不同检测方法提供统一比较，但数据生成器与人工过滤偏差仍需审计。
