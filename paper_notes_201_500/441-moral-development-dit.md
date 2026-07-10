# 441. Probing the Moral Development of Large Language Models through Defining Issues Test

- **Authors:** Kumar Tanmay, Aditi Khandelwal, Utkarsh Agarwal, Monojit Choudhury
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2309.13356
- **Tags:** `moral-reasoning` `DIT` `alignment` `behavioral-evaluation`

## Introduction
论文不把 alignment 简化成是否拒绝有害请求，而是借用道德心理学检验 LLM 能否在价值冲突的 dilemma 中进行分层道德推理。作者采用 Kohlberg Cognitive Moral Development 与 Rest 的 Defining Issues Test（DIT），因为它要求模型不仅选“应该/不应该/无法决定”，还要在 12 个伦理考虑中评分并排序，从而区分个人利益、社会规范和后传统原则。

## Method / Framework
每个 dilemma 提供 12 条对应不同 Kohlberg stage 的考虑，模型先回答 resolution，再按重要性评分并选出 top-4。核心 P-score 对排名前四项中属于 Stage 5/6 的条目按 4、3、2、1 加权再乘 10；高 P-score 表示更重视普遍正义、人权和原则，而不是简单服从规则。作者还计算 stage-wise score、Personal Interest、Maintaining Norms 和“cannot decide”比例。

评测包含 5 个 DIT-13 dilemma，加上作者设计的 Monica（论文署名）、Timmy（婚礼 vs 隐私风险 bug）、Rajesh（饮食与邻里）、Auroria（资源共享）四个跨文化/现代场景；每个 dilemma 使用 8 种 statement permutation 与 6 种 option permutation 共 48 个 prompt，temperature=0，以检查顺序偏差。

## Baselines / Comparisons
模型包括 GPT-3、GPT-3.5、GPT-4、两个时间版本的 ChatGPT、PaLM-2 和 Llama2-Chat 70B。比较基线是随机排序得到的 dilemma-specific P-score：三项 Stage 5/6 的 dilemma 为 25，四项的为 33.33，整体平均随机基线为 30.56；结果还与 DIT 人类群体区间比较，初高中约 20-30、成人/大学生约 40-46、研究生约 53-63。

## Experiments / Findings
GPT-3 平均 P-score 29.84，接近随机；GPT-4 达到 55.68，落在研究生早期后传统道德区间。GPT-3.5、ChatGPT、PaLM-2、Llama2-Chat 大体落在成人/大学生的 conventional 区间；PaLM-2 在可回答的 8 个 dilemma 上为 52.24，Llama2-Chat 也显著高于随机。整体趋势是模型更新、instruction/RLHF 能力和规模提升后更能遵守复杂格式并选择原则性条目。

关键反例是稳定性：Prisoner 和 Webster 等 dilemma 上多数模型都表现差，9 个 dilemma 中有 2 个没有任何模型显著超过随机；GPT-4 在新设计的 Rajesh dilemma 上甚至不如多数模型。violin plot 还显示同一模型不同排列/运行的答案差异明显，说明单次“道德回答”不能当作稳定的内部伦理能力。

## Ablation / Error Analysis
statement 和 option permutations 直接测试 position bias；GPT-3 经常固定选择第 1、3、5、7 位置，导致即使懂某些内容也不能正确完成多阶段指令。排除无法回答的 Doctor dilemma 后再比较，PaLM-2 的平均分仍受不同 dilemma 可答性影响；GPT-4 的高平均值与困难场景的失效并存。

DIT 的 P-score 只看排名中 Stage 5/6 条目，无法区分模型是否真正理解故事、是否利用训练语料中的道德模板，也不能覆盖文化、情感直觉或实际行为后果。作者自己将该测试视为 behavioral probe，而非模型拥有“道德人格”的证据。

## Limitations
DIT 本身有文化和性别偏差等心理测量争议，Kohlberg 阶段不能代表所有社会的道德观。模型是黑盒，GPT-4 训练数据/对齐过程未知，四个新 dilemma 的人工设计和 stage 标注也可能带来研究者偏差；P-score 与人类区间的类比不能直接推出部署安全性或替代人类裁决。

## 两句话总结
本文用 DIT 把 LLM 道德评测从单一偏好答案提升为伦理考虑的评分、排序和发展阶段分析，发现 GPT-4 达到研究生级 P-score，而 GPT-3 接近随机。更重要的是，模型在不同 dilemma 上不一致且对顺序和场景敏感，因此高平均道德分不能证明稳定、可迁移的道德推理能力。

## Evidence note
已逐段读取本地 PDF，核对了 DIT/P-score 定义、9 个 dilemma、48 种排列 prompt、7 个模型、随机基线 30.56、GPT-3 29.84、GPT-4 55.68 及困难 dilemma 分析。
