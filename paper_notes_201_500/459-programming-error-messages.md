# 459. Using Large Language Models to Enhance Programming Error Messages

- **Authors:** Juho Leinonen, Arto Hellas, Sami Sarsa, Brent Reeves, Paul Denny, James Prather, Brett A. Becker
- **Venue / year:** SIGCSE 2023
- **Paper:** [arXiv:2210.11630](https://arxiv.org/abs/2210.11630)
- **Tags:** `LLM-explanation` `code-education` `error-messages` `human-evaluation`

## Introduction
编译器错误信息面向熟练程序员，初学者常难以理解原因和修复方向。作者研究 Codex 能否把 Python 3.6 中最难读的九类 error message 改写为 plain-English explanation，并给出可执行的修复建议。

## Method / Framework
选取 `can't assign to function call`、`invalid token`、缩进、参数顺序、EOF、字符串和 unicode escape 等九类错误；每类构造 simple、含字符串/函数、使用 library 的三种程序。用 code-davinci-002 比较五种 prompt，空响应最少的 prompt 是“用 plain English 解释为何报错以及如何修复”。每个组合生成 temperature 0 一个、temperature 0.7 两个输出，共 81 个输出，由两名有教学经验的研究者判断 comprehension、是否解释、correctness、fix presence/correctness 和相对原消息的 improvement。

## Baselines / Comparisons
没有训练型模型 baseline，比较的是原始 compiler/interpreter message 与 Codex-enhanced message，并按 prompt、temperature、program complexity 和 error type 分层。文章也把结果放回传统 enhanced compiler error message 文献中，强调 LLM 方法减少人工重写成本但不自动保证正确。

## Experiments / Findings
平均 88% 输出可理解，84% 含解释，48% 的全部样本解释被判正确；54% 被认为比原消息更好。70% 输出含修复，只有 33% 的全部样本修复正确。`can't assign to function call` 的解释正确率最高 83%，`unexpected EOF while parsing` 只有 11%；temperature 0 通常优于 0.7。

## Ablation / Error Analysis
complexity 越高不一定更差，但 library/function-with-strings 组合会出现更多冗余或不准确内容。典型错误是把缺少代码误判为缩进问题，把大小写错误解释为 indentation，把缺少引号解释成括号错误；模型语气却很自信，可能让初学者形成错误概念。作者建议先按 error/context 风险筛选，再使用 LLM 并保留 human-in-the-loop。

## Limitations
只测 Python 3.6、九类人工挑选错误、81 个组合和两名评审，没有学生学习增益或真实 debugging success 的实验。旧 Python 的 error wording 与现代版本不同；“可理解”与“正确”也有主观评审差异。

## 两句话总结
Codex 能把多数难读的编程错误改写成通顺解释，约一半情形比原始消息更有用，但正确修复率只有 33%。论文最重要的警告是：LLM 解释的流畅度远高于事实可靠性，教育场景必须显示不确定性并采用教师/规则审查。

## Evidence note
已读取研究问题、九类错误构造、prompt/temperature 设计、双评审指标、表格结果、典型错误案例和 limitation/discussion。
