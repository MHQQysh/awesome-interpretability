# 027. Chain-of-Verification Reduces Hallucination in Large Language Models

- **作者 / venue**：Shehzaad Dhuliawala, Mojtaba Komeili, Jing Xu, Roberta Răileanu, Xian Li, Aslı Çelikyilmaz, Jason Weston；Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.212/)
- **任务**：用模型自生成的 verification questions 检查并修正初始答案

## 1. Introduction

LLM 可以生成流畅但错误的事实。让模型先写 chain-of-thought 有时提高推理，却不保证事实可靠；直接让模型“检查自己”也可能重复原来的错误。CoVe 的核心问题是：能否把 self-verification 变成一个独立、分阶段、减少 confirmation bias 的流程？

## 2. Method：四步 Chain-of-Verification

1. **Initial response**：模型先生成草稿答案。
2. **Verification planning**：根据草稿生成若干可单独验证的问题。
3. **Independent verification**：分别回答这些问题，避免把原答案直接放在上下文中导致自我确认。
4. **Final response**：根据验证结果重新生成最终答案。

论文还比较不同 verification 变体：有的把问题和原答案放在一起，有的采用独立 question answering，有的让所有问题共享上下文。核心控制变量是验证是否真正独立。

## 3. Baseline 与任务

- few-shot / standard prompting baseline。
- chain-of-thought 和 self-consistency 类 reasoning baseline。
- self-verification、deductive verification、训练/强化学习式 hallucination mitigation 方法。
- Wikidata list QA、long-form biography 等任务：从实体列表到长文本生成，覆盖不同 hallucination 形式。

## 4. Findings

- CoVe 在 list-based questions 上尤其有效，因为每个实体可以被拆成独立 verification question，再合并回最终列表。
- 论文指出 Llama few-shot baseline 在某些 Wikidata 列表任务中只有约 **17%** 的实体正确；逐实体验证可以显著提升正确率。
- CoVe 对长篇 biography 也减少具体事实错误，但效果取决于 verification question 是否覆盖真正风险点。
- 独立验证比把原答案作为唯一上下文更可靠，因为后者容易重复初始 hallucination。
- 代价是多次 LLM 调用和更长延迟；因此 CoVe 更像可靠性增强流程，而不是免费 decoding trick。

## 5. Ablation 与局限

验证问题数量、是否独立、是否允许检索以及最终整合 prompt 都会改变结果。CoVe 不能保证验证器本身不 hallucinate；对复杂关系事实、长文整体一致性和需要外部知识的问题仍可能漏检。

**一句话评价**：CoVe 把“自我反思”改造成可审计的草稿-问题-独立核验-重写管线，最关键的机制是打破初始答案对验证过程的污染。
