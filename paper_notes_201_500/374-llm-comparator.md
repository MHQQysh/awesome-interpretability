# 374. LLM Comparator: Visual Analytics for Side-by-Side Evaluation of Large Language Models

- **Authors:** Minsuk Kahng, Ian Tenney, Mahima Pushkarna, Michael Xieyang Liu, James Wexler, Emily Reif, Krystal Kallarackal, Minsuk Chang, et al.
- **Venue / year:** CHI Extended Abstracts, 2024
- **Paper:** https://arxiv.org/abs/2402.10524
- **Tags:** `LLM-as-a-judge` `visual-analytics` `evaluation` `rationale` `human-in-the-loop`

## Introduction
LLM-as-a-judge/automatic side-by-side (AutoSxS) evaluation通常给出一个模型相对 baseline 的平均分或 win rate，并附带 judge 的简短 rationale。这个数字适合快速筛选模型，却很难回答三个解释性问题：模型 A 在什么场景赢、judge 为什么认为 A 赢、A 和 B 的文本到底哪里不同。

作者先与大型科技公司多个团队的 20 多位研究者和工程师交流，发现实践中大家仍把结果导入 spreadsheet 或 notebook，再手动抽样看 prompt-response 对；聚合指标和单例阅读之间缺少连接。LLM Comparator 的目标是让“when / why / how”三个层次的分析在同一个交互界面完成，而不是声称自动 judge 本身已经可靠。

## Method / Framework
输入是一组 prompt、模型 A/B 的 response、一个或多个 judge 的 rating 和 rationale，以及 prompt category。工具包含两部分：

- **Interactive table:** 每行显示 prompt、两份回答、平均 score 和 rationale summary；用重叠词高亮帮助快速比较文本，支持展开各个 judge 的详细结果，并用颜色区分 A 胜、B 胜和 tie。
- **Visualization summary:** score distribution 直方图；按 prompt category 展示 win rate；把 rationale bullet 聚成代表性主题；统计 n-gram；允许用户写正则/自定义函数计算文本特征。

Rationale clusters 不是直接对所有句子做黑箱聚类：先用另一个 LLM 从 rationale 样本生成多样且有代表性的 cluster labels，再用 embedding cosine similarity 将 bullet 分配到一个或多个主题。用户可以增删主题、对过滤后的样本重新聚类，并联动 category 和表格查看原始证据。

## Baselines / Comparisons
论文没有把某个模型当作新的预测 baseline，也没有训练新的 judge。它的“baseline”是当前 AutoSxS 工作流：baseline model 通常是线上版本或 PaLM 2 等已有强模型；judge 返回 Likert rating，重复多次取平均，win rate 以 score > 0.3 或 < -0.3 判定。

因此评价重点是 workflow 对比而非 accuracy 对比：传统 spreadsheet/notebook + 人工抽样， versus LLM Comparator 的聚合-单例联动。论文还用自然语言生成模型完成 rationale summary 和 cluster label，这增加了一个解释层，不能把 summary 当成原始 judge 证据。

## Experiments / Findings
工具已经接入 Google 的评估流程，最初三个月吸引超过 400 位用户，并分析超过 1,000 个自动 side-by-side 实验。这个部署规模说明问题来自真实评测，而不是仅由作者构造的 toy demo。

定性用户研究邀请 6 位经常使用 automatic raters 的参与者。设计任务围绕：定位模型何时胜负、理解 judge 的共同理由、比较两个 response 的语言差异。参与者能够从 category 或 score distribution 进入代表性样本，再回到表格查看原始 prompt/response；n-gram 和 custom function 让“更 concise”“更 organized”等模糊 rationale 变成可进一步核查的文本模式。

## Ablation / Error Analysis
论文不是传统 ablation study，因此没有报告“去掉某组件后准确率下降多少”。可视为 workflow 级消融的组件包括 score distribution、category slices、rationale clusters、n-grams/custom functions 和 interactive table；它们分别覆盖 when/why/how，缺一项就会迫使用户回到手工脚本。

关键误差边界是 judge bias 和 summary error：rationale 可能本身错误或不一致，第二个 LLM 的摘要/标签还可能压缩掉限定条件。相似度阈值是经验确定的，cluster 也允许一条 bullet 归入多个主题，因此主题计数应被理解为探索线索，不是统计显著的因果结论。

## Limitations
研究参与者数量为 6，且来自同一大公司生态，用户结论主要是定性反馈。工具依赖已有 AutoSxS judge 的质量，不能解决 position bias、verbosity bias、judge disagreement 或 reference-free evaluation 的基础问题。rationale summary 和 cluster label 引入第二层 LLM 解释，仍需要用户回看原始 response/rationale；论文也没有用人工金标准定量证明工具提高了模型选择正确率。

## 两句话总结
LLM Comparator 把 AutoSxS 的分数、切片表现、judge rationale 和两模型原文放进同一个 visual analytics workflow，使研究者能从 when、why、how 三个维度追查模型差异。它的贡献是可用的评测解释界面而不是新的 judge 算法，真正可信的结论仍必须回到原始回答和人工判断。

## Evidence note
已读取 arXiv 2402.10524/CHI 版本本地 PDF 文本；部署规模、20+ 访谈背景、6 人定性研究和各可视化组件均来自论文正文。
