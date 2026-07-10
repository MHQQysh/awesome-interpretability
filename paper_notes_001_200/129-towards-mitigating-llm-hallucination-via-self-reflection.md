# 129. Towards Mitigating Hallucination in Large Language Models via Self-Reflection

> 人工精读笔记：Findings of EMNLP 2023。重点是 interactive self-reflection loop 在医学 QA 中逐步获取知识、核对答案和修订。

## 0. 论文信息

- **作者**：Ziwei Ji, Tiezheng Yu, Yan Xu, Nayeon Lee 等
- **来源**：[Findings of EMNLP 2023](https://aclanthology.org/2023.findings-emnlp.123)
- **任务**：MedNLI、PubMedQA、医学长答案

## 1. Introduction：要解决什么问题

医学 hallucination 往往“听起来合理但事实错误”，例如把 PTEN mutation 与 Noonan syndrome 错误关联。一次性生成无法区分知识缺失、问题理解错和答案跑题；需要让模型自我检查 factuality、relevance 和 consistency。

## 2. Method / Framework：怎么解决

方法包含 factual knowledge acquiring loop、question-entailment answering loop 和 answer refinement。模型先生成候选答案，再以不同 aspect/score prompt 反思支持证据、问题一致性和切题性，最后重新生成更可靠答案；可通过 LoRA 在医疗数据上适配。

## 3. Baseline / 对比基线

直接 generation、MedAlpaca、Alpaca-LoRA、不同 LLM/参数规模、无 refinement、无 aspect score、无 exact score number 的 self-reflection。

## 4. Experiments / Findings：结果如何读

在 MedNLI/PubMedQA 和长答案评估中，self-reflection loop 改善 factuality、sentence-level consistency 和 relevance，自动指标比直接 generation 更好；人工评价也显示答案更少出现 unsupported claim。该方法在不同规模模型上都有效，尤其在医学领域知识不足的模型上。

## 5. Ablation / 机制解释

去掉 refinement、去掉 aspect、只给文字 feedback、提供具体 score number 都会改变结果。完整 loop 通常最好，但精确分数未必必要；反思的多维检查比单一“是否正确”更能发现切题和事实问题。

## 6. Limitations / 局限与复现注意

模型自我评价可能与真实医学事实共同错误；多轮调用成本高；反思 prompt 可能引入过度谨慎/拒答；医学 benchmark 不能代表通用开放生成。

## 7. 两句话总结

论文让 LLM 通过知识获取、问题蕴含检查和答案 refinement 的循环减少医学 hallucination。自动和人工实验均优于直接生成，但 self-reflection 仍依赖外部事实可验证性和模型自身的判断能力。
