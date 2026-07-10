# 402. Unelicitable Backdoors in Language Models via Cryptographic Transformer Circuits

- **Authors:** Andis Draguns, Andrew Gritsevskiy, Sumeet Ramesh Motwani, Charlie Rogers-Smith, Jeffrey Ladish, Christian Schroeder de Witt
- **Venue / year:** arXiv, 2025
- **Paper:** https://arxiv.org/abs/2406.02619
- **Tags:** `mechanistic-interpretability` `backdoor` `cryptographic-circuit` `model-safety`

## Introduction
传统 backdoor 往往有容易猜到或优化触发器，防御者可以通过 elicitation、red teaming 或输入扫描触发异常行为。本文构造一种更难检测的 **unelicitable backdoor**：只有持有密码学秘密 trigger 才会改变输出，其他输入以高概率与 clean model 完全一致，因此黑箱行为测试很难发现。

## Method / Framework
作者把 encrypted payload 嵌入 Transformer circuit，用密码学函数在 token/hidden representation 中计算 trigger。设计目标包括 universal、robust、stealthy 和不改变无 trigger 行为；触发时可以令模型输出预设不安全行为。论文提供 Python library 和白箱构造，并依赖 SHA-2 等抗 preimage 假设讨论 unelicitable 性质。

## Baselines / Comparisons
对比传统 backdoor、elicitation-based mitigation、ELK/monitoring 类 honesty 方法和白箱检测。评价维度是 trigger 是否能触发 payload、无 trigger 时 clean/backdoored model 是否等价、对 state-of-the-art mitigation 是否有抵抗，以及白箱下是否仍留下可识别结构。

## Experiments / Findings
实验证明构造出的 trigger 对 defender 的普通搜索几乎不可 elicitation，且高概率不改变非触发输入。作者还报告对现有 mitigation 有较强抵抗；白箱条件下并非数学意义上的完全不可见，但比普通 backdoor 更难区分。该结果把“只做行为 red team”对安全的信心边界暴露出来。

## Ablation / Error Analysis
论文比较加密 payload、trigger 长度/结构、Transformer module placement 和 clean/backdoor 输出差异。密码学假设、近似实现和模型容量决定成功率；如果攻击者拿到权重，专业分析仍可能检测异常。失败或误报的来源包括触发器 tokenization、模型本身对异常输入的响应和密码电路可见的参数 footprint。

## Limitations
这是安全构造和威胁论证，不代表现实攻击者已经能在任意大模型供应链注入该电路。秘密管理、训练可行性、模型规模、白箱审计和后门 payload 的复杂度都有工程限制。机制可解释性在这里既是防御工具，也是攻击者隐藏/构造电路的双刃剑。

## 两句话总结
本文用密码学 Transformer circuit 构造只有秘密 trigger 才激活的 unelicitable language-model backdoor，挑战依赖普通 elicitation 和行为测试的部署前审计。它说明“没有找到触发器”不能证明模型没有后门，必须结合白箱电路分析、供应链验证和密码学威胁模型。

## Evidence note
已读取 arXiv 2406.02619 v2 本地 PDF 的摘要、构造、威胁模型和实验讨论；具体攻击 payload 不在笔记中复现。
