# 378. LLMs Will Always Hallucinate, and We Need to Live With This

- **Authors:** Sourav Banerjee, Ayushi Agarwal, Saloni Singla
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2409.05746
- **Tags:** `LLM` `hallucination` `theory` `uncertainty` `limitations`

## Introduction
许多 hallucination 工作把目标设为更好的数据、架构、检索、事实核验或 uncertainty estimation，从而降低错误率。本文提出一个更强、也更具争议的理论命题：只要 LLM 仍是有限训练数据上的通用计算系统，并承担开放式语言生成，就不可能把 hallucination 从根本上完全消除；工程目标应转为检测、约束和与不确定性共存。

论文不是提出新的 detector 或 benchmark，而是把“为什么会错”提升到可计算性和逻辑结构层面。阅读时需要把它看作作者的 impossibility argument，而不是已经由经验实验完全证实的定理。

## Method / Framework
作者将 hallucination 统一描述为 structural hallucination，并依次讨论：

1. **Training incompleteness:** 训练数据不可能包含所有事实、组合和未来事件，模型遇到分布外或未见问题时必须泛化/猜测。
2. **Needle-in-a-haystack:** 即使答案存在于训练或上下文，有限表示和检索过程也可能无法可靠取出；记住局部事实不等于稳定可检索。
3. **Intent/semantic classification:** 论文借助不可判定性结果说明，开放输入的意图、真值或任务类别不可能被一个通用有限程序对所有输入完美判断。
4. **Generation/halting:** 生成器和停止条件也受到 Halting/Acceptance/Emptiness 等问题的结构限制，不能保证每一次输出都满足外部事实约束。
5. **Fact checking insufficiency:** 外接 fact checker 只能检查可表达、可检索且 checker 自己能判定的命题，不能消除所有错误来源。

## Baselines / Comparisons
论文将其观点与常见 mitigation 方向作概念对比：扩大数据和模型、RAG、self-consistency、CoT、uncertainty estimation、human feedback、外部事实核验和更好的 decoding。它没有按统一数据集报告这些方法的 accuracy 表，也没有实现一个新的 baseline；比较的是“降低已观察错误的概率”与“对所有输入保证零 hallucination”这两个不同目标。

这一点很重要：作者的结论并不是 mitigation 没有效果，而是 mitigation 不能把开放世界生成变成绝对可靠的 oracle。任何声称完全消除 hallucination 的系统都必须隐含限制输入域、事实源、时间范围或输出空间。

## Experiments / Findings
主要 findings 是理论性而非经验性：训练集有限意味着知识覆盖不完备；通用 intent classification 和某些生成/验证问题可归约到不可判定问题；因而存在某些输入，系统无法在有限资源下保证正确回答或可靠判定。论文用随机五词句子和“needle”式场景说明，表面上流畅的输出可以没有稳定语义依据。

作者还区分“模型能在多数 benchmark 上表现很好”和“模型对所有可能输入都不会 hallucinate”。前者可以通过规模、检索和提示显著改善，后者是更强且不现实的安全保证。对可解释性来说，CoT 或 uncertainty token 是行为证据，不是一个完备的内部真值证明。

## Ablation / Error Analysis
本文没有 conventional ablation。其等价的反事实分析是逐一去掉假设：即便增加训练数据仍不覆盖无限开放问题；即便使用 self-consistency，多条样本也可能共享同一错误；即便使用 CoT，推理链可能是事后生成的；即便使用 fact checker，也会遇到 checker 自身错误、无法检索、命题不可判定或事实随时间变化。

论文的主要风险在论证边界：从“存在不可判定/不可覆盖的输入”推到“每个实际 LLM 必然在某些常见任务上 hallucinate”，需要明确模型、输入分布和 hallucination 定义。它提供的是结构性警告，不是对具体模型 hallucination rate 的测量。

## Limitations
文章缺少形式化的统一模型定义和可复现实验，因此无法给出不同 mitigation 的实际收益、错误率或任务条件。将计算理论结果应用到现代随机神经网络和开放式语义时，需要额外的归约假设；“不可完全消除”也不等于“无法在给定受限域内提供高可靠性”。它对产品级检测、拒答阈值和人机协作策略讨论较少。

## 两句话总结
本文用训练不完备、检索失败和不可判定性论证，主张开放式 LLM 生成中存在无法彻底消除的结构性 hallucination，因此工程重点应从绝对清零转为检测、约束和风险管理。它没有提供新的实证算法，且其强结论依赖理论假设，不能替代对具体模型、数据分布和 mitigation 的实测。

## Evidence note
已读取 arXiv 2409.05746 本地 PDF 的摘要、理论主张、不可判定性章节和结论；文中明确区分作者的理论论题与经验性 hallucination benchmark。
