# 386. Large Language Models Cannot Explain Themselves

- **Authors:** Advait Sarkar
- **Venue / year:** CHI position/survey paper, 2024
- **Paper:** https://arxiv.org/abs/2405.04382
- **Tags:** `LLM` `rationale` `faithfulness` `human-AI-interaction` `explanation`

## Introduction
LLM 可以被提示去解释自己的答案，因此产品常把一段生成的 rationale 当作 transparency feature。本文的中心论点是：这种文字通常不是对模型机械预测过程的忠实报告，因为模型生成 explanation 和生成 answer 使用的是同一种 next-token mechanism，模型并没有直接访问一个可读的因果 trace。

问题因此不只是“解释错了”，而是用户会把 fluent、具体且自信的文本误认为模型真实 reasoning，产生过度信任、错误依赖和不当责任归因。作者也不主张所有 rationale 都没有用，而是区分帮助用户反思的 explanation 与声称揭示内部机制的 explanation。

## Method / Framework
这是一篇概念/position paper，区分三种常被混在一起的东西：

- **mechanistic explanation:** 描述真实产生输出的内部组件、计算和因果路径；
- **rationale:** 模型事后给出的自然语言理由，可能与答案相关但不一定是答案的原因；
- **user-facing explanation:** 即使不是机制真相，也可能通过展示证据、假设或不确定性帮助用户批判性思考。

作者将 LLM explanation 放在 human-AI decision support 和 HCI 的“explanations affect mental models”传统中，强调应设计 provenance、uncertainty、counterevidence 和 user verification，而不是只增加更多语言。

## Baselines / Comparisons
论文没有新模型或统一实验。概念对比包括模型自述 rationale、外部 feature attribution/visualization、可验证的程序/检索证据、传统可解释模型和真正的 mechanistic interpretability。一个完全透明的 symbolic pipeline 可以提供因果步骤；LLM 自述更像 post-hoc narrative，必须通过 input perturbation、activation intervention 或外部 evidence 检查。

## Experiments / Findings
文章的主要 finding 是解释的社会效用和机制忠实性可以分离：一个 rationale 可能让用户更容易理解、记住或质疑答案，但仍然不能回答“哪些权重/激活导致了这个 token”。因此“解释是有用的”不等于“解释是真实的”。

作者特别警惕 fluent false explanation：模型可以为错误答案生成连贯理由，甚至引用不存在的证据；用户看到具体理由后反而提高错误信心。对产品设计而言，最可靠的界面不应只显示 self-explanation，还应显示来源、可验证中间产物、置信度和明确的未知状态。

## Ablation / Error Analysis
本文没有 conventional ablation。其反事实建议包括：改变 prompt/答案而保持解释请求，检查 rationale 是否跟随答案；遮蔽被解释为关键的输入，检查预测是否变化；比较 rationale 与 activation/feature attribution；让另一个模型或人类检查理由是否引用真实证据。

错误分析的根源是“生成 explanation 的模型没有 privileged access to its own computation”。即便 reasoning token 与正确答案相关，它们也可能是模型学到的解释风格；即便 explanation 与输出一致，也不能排除另一个未报告的机制才真正产生输出。

## Limitations
文章是警示性论证，不提供一个新型 faithful explanation 算法，也没有统一量化不同 rationale 失真的概率。mechanistic explanation 本身在大模型上也很难，外部 attribution 同样可能有局限。作者的“不能 explain themselves”应理解为不能仅靠自由生成的自述保证机制忠实，不是说 LLM 输出的所有解释都没有用户价值。

## 两句话总结
本文指出 LLM 自己生成的 rationale 通常是 post-hoc text，不应被当作准确描述 next-token 预测的机械因果过程。更安全的解释设计应把 rationale 当作辅助沟通层，并用证据、干预、反事实和不确定性校验其可靠性。

## Evidence note
已读取 arXiv 2405.04382 本地 PDF 的摘要、概念区分、HCI 风险与结论；本文没有实验表，已明确标为 position/conceptual work。
