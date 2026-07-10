# 450. Faithful Chain-of-Thought Reasoning

- **Authors:** Qing Lyu, Shreya Havaldar, Adam Stein, Li Zhang, Delip Rao, Eric Wong, Marianna Apidianaki, Chris Callison-Burch
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2301.13379
- **Tags:** `faithful-reasoning` `chain-of-thought` `program-aided-reasoning` `code-interpreter`

## Introduction
普通 CoT 让模型写自然语言步骤，但自然语言步骤可能算错、被忽略或无法执行；即使最终答案对了，也不能确认每个中间结论真的支持答案。本文把 CoT 变成“自然语言分解 + 可执行符号程序”：模型负责提出 subquestions、依赖图和 rationales，确定性 interpreter/planner 负责执行代码并得到最终答案。

## Method / Framework
Faithful CoT 的 Translation stage 生成交错的 `C_NL` 与 `C_SL`。`C_NL` 明确列出 subquestion、independent/dependent dependency 和 support rationale；`C_SL` 用 Python、Datalog 或 PDDL 表达计算。Problem Solving stage 不再让 LM 自己算最终答案，而是调用 Python interpreter、Datalog relational engine 或 PDDL planner 执行程序。

论文覆盖四类任务：GSM8K/SVAMP/MultiArith/ASDiv/AQuA 数学题，StrategyQA/Date/Sports 多跳 QA，SayCan household planning，CLUTRR family-relation inference。一个 arithmetic example 中普通 CoT 把 2 小时误算成 120 分钟，Faithful CoT 生成 `minutes_rob=2*60`、`minutes_royce=minutes_rob*2+40`，interpreter 得到 280；答案和中间步骤都可检查。

## Baselines / Comparisons
比较 standard prompting、CoT 和 Least-to-Most（LtM），同时测试 greedy 与 self-consistency decoding（temperature 0.4、40 samples）。主模型为 Codex `code-davinci-002`，附录还换了其他 code-generation LM；指标是各任务 final-answer exact accuracy，并额外做人工 reasoning-chain correctness/plausibility 评估。

## Experiments / Findings
greedy 结果中，Faithful CoT 在 GSM8K/SVAMP/MultiArith/ASDiv/AQuA/SayCan/StrategyQA/Date/Sports/CLUTRR 分别为 72.3/83.4/98.8/80.2/47.2/89.3/63.0/81.6/99.1/58.9；普通 CoT 为 63.3/77.3/96.5/80.0/42.1/86.4/72.5/59.9/98.6/48.5。它在大多数任务超过 CoT/LtM，尤其数学和 CLUTRR；StrategyQA 是例外，CoT 的开放域知识和事实检索更强。

人工评估显示 reasoning chain 的可执行结构便于检查和人机协作：GSM8K 错误中 49% 是 wrong NL subquestion、24% wrong code、12% semantic understanding error、3% missing subquestion，很多 wrong subquestion/code 可以被人直接修改。CLUTRR 错误主要是 inverse relation（41%）和额外 subgoals（64% of annotated Faithful-CoT errors），比“不可解释的最终数字错”更容易定位。

## Ablation / Error Analysis
执行式程序是核心 ablation：只写 NL 的 LtM 不能保证子答案被真正计算，加入 symbolic language 才能让 solver 检查依赖和执行顺序。不同 domain 使用不同 DSL 也影响 failure：Datalog 的关系方向、PDDL 的 goal/plan、Python 的变量与单位各有错误模式；数据清洗还发现 ASDiv、Date、Sports、SayCan 存在标签、歧义或环境设定问题。

人类评估的正确率并非所有域都高：MWP 92%、Planning 94%、StrategyQA 66.7%，后者的 30.3% 被标为 incorrect NL，说明可执行格式不能自动解决知识和语义理解。Faithful CoT 在 CLUTRR 只产生 7 个错误，但其中多是额外 subgoals 把规划带偏，展示了“可读但不必要的推理”这一新 failure mode。

## Limitations
方法需要为每个领域设计 DSL、interpreter/planner 和 prompt，工程成本高；solver 只能保证生成程序的执行一致性，不能保证 LM 提出的事实、subquestion 或 rationale 本身正确。代码执行还有安全边界，开放域知识、模糊目标和真正连续控制不能简单转成确定性程序；最终仍需人工/外部验证 rationale 与现实的对应关系。

## 两句话总结
Faithful CoT 让 LM 负责可读的任务分解，让确定性程序执行中间计算，从结构上减少“自然语言步骤看似合理但实际没被用到”的问题。它在多类数学、规划、关系推理任务上超过普通 CoT，并把错误暴露为可编辑的 subquestion/code/DSL 错误，但代价是每个领域都要构造符号语言和可靠 solver。

## Evidence note
已逐段读取本地 PDF，核对了 Translation/Problem Solving 两阶段、Python/Datalog/PDDL、10 个数据集、standard/CoT/LtM/自一致性基线、Table 1 数值和人工错误分类。
