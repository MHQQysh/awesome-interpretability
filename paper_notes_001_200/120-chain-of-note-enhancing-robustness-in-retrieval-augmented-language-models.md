# 120. Chain-of-Note: Enhancing Robustness in Retrieval-Augmented Language Models

> 人工精读笔记：EMNLP 2024。重点是让 RALM 先对 retrieved documents 写 reading notes，再决定利用、忽略或拒答。

## 0. 论文信息

- **作者**：Wenhao Yu, Hongming Zhang 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.813)
- **方法**：CHAIN-OF-NOTE（CON）
- **模型/数据**：Llama-2 7B、GPT-4；NQ、TriviaQA、TimeQA 等

## 1. Introduction：要解决什么问题

RAG 依赖 retriever，但 top-k 文档可能包含噪声、互相冲突或无法回答问题的内容。普通 retrieve-then-read 容易把 irrelevant passage 当证据，产生 hallucination；没有知识的问题也可能被迫从错误 context 生成答案。

## 2. Method / Framework：怎么解决

CON 先让模型逐篇读取 retrieved documents，生成 notes，判断文档提供了什么、是否相关、是否足够回答，再综合 notes 形成最终 answer。notes 作为显式中间状态，鼓励模型区分 direct retrieval、inferential reasoning 和 knowledge boundary。

推理时可结合直接生成、notes-only 和 hybrid strategy；训练/提示主要在 Llama-2 7B 上实施，并用噪声比例、unknown questions 和拒答率评估 robustness。

## 3. Baseline / 对比基线

普通 RALM retrieve-read-answer；无 retrieval 的 QA fine-tuning/closed-book；CoT；SAIL 等 retrieval-augmented instruction tuning；GPT-4 zero-shot 与 CON；不同 noisy document ratio。

## 4. Experiments / Findings：结果如何读

在 noise robustness 上，加入 irrelevant documents 后 CON 的性能下降更小，能接近无检索 baseline；它通过 notes 过滤噪声，而不是盲目把 top-k 全部塞给 answer generator。不同 noise ratio 下优势总体稳定。

在 unknown robustness 的 TimeQA 上，CON 更愿意识别自身缺少知识并拒答/降低错误生成；对模型训练域外的实时信息尤其重要。普通 RALM 有时因错误 retrieval 反而比 closed-book 更差，CON 能缓解这种 retrieval-induced hallucination。

CON 会增加推理时间，因为每篇文档都要写 note；表中显示计算成本上升但在 Llama-2 7B 上仍可接受。GPT-4 + CON 在多项 robustness 设置超过 Llama-2 7B，而 hybrid 方法在部分 noise ratio 下有 trade-off。

## 5. Ablation / 机制解释

直接 answer vs notes、notes-only vs hybrid、不同 noise ratio、是否允许 reject、不同 note prompt 和模型规模构成主要对照。结果支持中间 reading note 的作用，但也显示 note 过长会增加延迟并可能引入新的错误。

## 6. Limitations / 局限与复现注意

notes 仍由 LLM 生成，可能把错误 retrieval 总结得更可信；开放域 QA 的 exact match/F1 不完全反映拒答质量；多篇文档逐篇处理成本高；真实多跳 RAG 还需验证。

## 7. 两句话总结

Chain-of-Note 让 RALM 在最终回答前逐篇判断 retrieved context 的相关性、信息充分性和知识边界，从而提升噪声和未知问题鲁棒性。实验显示它比普通 retrieve-read 更少受 irrelevant documents 影响，但额外 notes 也带来推理成本和“错误总结被放大”的风险。
