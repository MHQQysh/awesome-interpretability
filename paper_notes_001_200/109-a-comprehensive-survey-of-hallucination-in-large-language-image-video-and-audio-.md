# 109. A Comprehensive Survey of Hallucination in Large Language, Image, Video and Audio Foundation Models

> 人工精读笔记：Findings of EMNLP 2024 survey。重点是跨文本、图像、视频、音频 foundation model 的 hallucination taxonomy、检测和缓解。

## 0. 论文信息

- **作者**：Pranab Sahoo 等
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.685)
- **范围**：language、vision、video、audio foundation models；文献截至 2024 年 5 月

## 1. Introduction：要解决什么问题

Hallucination 不只发生在文本 LLM：模型可能生成与事实、输入图像、视频时间关系或音频内容不一致的输出。不同模态使用不同术语和指标，研究容易把 object hallucination、knowledge hallucination、temporal hallucination 与 reasoning error 混为一谈。

survey 的目标是统一定义与 taxonomy，按 detection/evaluation 和 mitigation 两条线梳理各模态方法，并指出哪些问题来自模型内部知识、哪些来自 multimodal alignment、数据噪声或 decoding。

## 2. Method / Framework：怎么组织综述

### 2.1 Detection/evaluation

文本侧包括 SelfCheckGPT、FActScore、NLI/entailment、claim decomposition、外部检索和 uncertainty；视觉侧包括 CHAIR、POPE、NOPE、object/attribute relation evaluation；视频侧加入 temporal consistency；音频侧检测对象、事件和语音内容 hallucination。

### 2.2 Mitigation

方法分为 data curation/fine-tuning、alignment/preference optimization、retrieval/grounding、prompt/decoding、model editing、architecture/memory 和 post-processing。多模态中还需处理视觉 instruction 数据中的 hallucinated objects、跨模态对齐和时间记忆。

## 3. Baseline / 对比维度

- **Intrinsic vs extrinsic hallucination**：输出违背输入/证据，或输入没有要求而模型凭空添加知识。
- **Text-only vs multimodal**：文本事实性不能直接代表视觉/视频/音频 groundedness。
- **Detection vs mitigation**：发现错误与真正降低错误是不同目标。
- **Automatic vs human evaluation**：自动 metric 可扩展但可能采用浅表启发式，人工判断成本高但更接近真实风险。
- **Data/fine-tuning/retrieval/decoding**：分别从训练分布、外部知识、生成过程和模型结构缓解。

## 4. Findings：综述得到的主要结论

文本 hallucination 常分为 factual、faithfulness、reasoning、contextual 和 citation 类型；视觉模型除知识幻觉外还会生成不存在对象、错误属性/关系，视频模型会漏掉时序和动作因果，音频模型可能幻觉对象、事件或情绪。

统一的核心规律是：模型规模和语言流畅度不能保证 groundedness；外部知识、视觉/音频输入、长上下文和复杂推理都会引入新的失败模式。RAG/grounding 能降低部分错误，但 retrieved source 不完整或有冲突时仍会 hallucinate。

检测方面，单一指标不够：FActScore/claim entailment 更适合事实，CHAIR/POPE 更适合对象，CLIP/BLEU 等相似度指标可能和人类 hallucination 判断弱相关。缓解方面，数据清洗、检索证据、contrastive/preference training 和 decoding control 各有 trade-off，且跨模态标准 benchmark 仍缺失。

## 5. Ablation / 机制解释

survey 中的对比主要是跨方法和模态的 protocol comparison：有无外部证据、有无视觉 instruction 扩充、有无 uncertainty/decoding penalty、不同数据和模型规模。论文强调要同时报告 hallucination rate、任务质量、引用正确性和计算成本，不能只展示一个 mitigation 分数。

## 6. Limitations / 局限与复现注意

- 综述覆盖广但截至时间固定，快速发展的多模态模型会迅速改变结论。
- 不同模态的 hallucination 定义和标注不可完全统一。
- 许多研究使用 GPT judge 或合成数据，存在 evaluator bias 和数据污染。
- survey 汇总既有结果，不重新统一 benchmark，因此表格数字不能直接形成总 leaderboard。

## 7. 两句话总结

这篇 survey 统一梳理文本、图像、视频和音频 foundation model 的 hallucination 类型、检测指标和缓解路线，强调 hallucination 是跨模态但机制不完全相同的可靠性问题。它的主要结论是没有单一 metric 或 mitigation 能覆盖所有错误，未来需要 grounded、可解释、跨模态且标准化的评测。
