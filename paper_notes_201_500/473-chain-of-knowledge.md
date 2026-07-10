# 473. Boosting Language Models Reasoning with Chain-of-Knowledge Prompting

- **Authors:** Jianing Wang, Qiushi Sun, Xiang Li, Ming Gao
- **Venue / year:** arXiv 2023/2024
- **Paper:** [arXiv:2306.06427](https://arxiv.org/abs/2306.06427)
- **Tags:** `chain-of-knowledge` `rationale-faithfulness` `verification` `prompting`

## Introduction
普通 CoT 的中间句子可能是幻觉或逻辑上“看起来合理但不真实”，错误证据会把最终答案一起带偏。Chain-of-Knowledge (CoK) 让模型先列出结构化 evidence triples，再用 F2-Verification 检查 factuality/faithfulness，并把错误证据标出来让模型重想。

## Method / Framework
CoK prompt 将 reasoning chain 表示为 `(subject, relation, object)` 结构三元组，形成可检查的知识图。F2-Verification 从 evidence 的事实性和是否真正支持 final answer 两个维度评估；不可靠时将错误 evidence 反馈回 LLM 重新生成。实验覆盖 commonsense、factual、symbolic、arithmetic reasoning，并包含 domain adaptation 的知识注入。

## Baselines / Comparisons
比较 standard ICL、zero-shot CoT、few-shot CoT、self-consistency、带/不带 knowledge base 的 CoK、不同错误 demonstration 比例和六种 ablation。模型包括 text-davinci-002 与 gpt-3.5-turbo，任务含 StrategyQA/CSQA、FOLIO/事实、Last Letter Connection 和算术。

## Experiments / Findings
CoK 在 commonsense、factual、symbolic 和 arithmetic 上普遍超过 CoT，F2-Verification 还能进一步提高准确率；结构化 evidence 同时让 reasoning 更容易人工检查。domain adaptation 中把知识库注入 prompt 能把模型的链条从“自由猜测”转成对特定领域实体/关系的显式引用。

## Ablation / Error Analysis
去掉 evidence triples、只保留 final answer、去掉 F2 或不进行 rethinking 都会削弱收益；错误 demonstrations 比例增加时 accuracy 下降，但结构化 verification 的下降更慢。作者强调错误 chain 分为 factual evidence 错、faithfulness 错和最终组合错，单一答案准确率无法区分三者。

## Limitations
triples 需要关系抽取和 verifier 正确，开放文本不一定能自然表示为二元关系；verification 仍依赖 LLM/外部 KB，可能把表面一致当事实。prompt 规模、知识库覆盖和人工构造 demonstrations 也影响可扩展性。

## 两句话总结
CoK 把 CoT 变成结构化知识证据，并用 F2-Verification 发现错误后重新思考，减少“看似合理”的不忠实推理。它提升多类 reasoning 任务，但 evidence 抽取、KB 覆盖和 verifier 自身可靠性决定了最终收益。

## Evidence note
已读取 CoK/F2 定义、四类任务、主表、domain adaptation、错误 demonstrations 和 ablation/limitations。
