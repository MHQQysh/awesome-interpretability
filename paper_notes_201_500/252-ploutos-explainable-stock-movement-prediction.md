# 252. Ploutos: Towards Explainable Stock Movement Prediction with Financial Large Language Model

- **Authors:** Hanshuang Tong, Jun Li, Ning Wu, Ming Gong, Dongmei Zhang, Qi Zhang
- **Venue / year:** 2024 preprint; listed in the 2025 WWW-related collection
- **Paper:** https://arxiv.org/abs/2403.00782
- **Tags:** Financial LLM, Stock Movement, Faithful Rationales, Multimodal Data

## Introduction

股票涨跌预测同时依赖新闻/社交媒体文本和价格技术指标。传统时序、图模型可以处理数值信号，却很难灵活融合文本并给出可审计理由；直接 zero-shot 使用 GPT-4 或 LLaMA 又接近随机猜测。Ploutos 要解决的问题是把不同金融视角变成可组合的“专家意见”，并让最终 rationale 对输入事实忠实，而不是事后编造一段听起来合理的解释。

## Method / Framework

PloutosGen 是专家池，包含 sentiment analysis expert、technical analysis expert 和 human/fundamental analysis expert。情感专家用金融情感数据微调 LLaMA-2，并对过去 5 天新闻聚合；技术专家用 N2I-Align 把多种 alpha 因子、历史数值和它们的文字解释转成 LLM 可处理的 number-to-text 序列；基本面专家提供公司与市场背景。PloutosGPT 接收各专家的 bullish/bearish signals，输出二分类涨跌与按重要性排序的理由。

训练阶段有两个关键设计：rearview-mirror prompting 让 GPT-4 根据历史输入、答案和分析生成更高质量的监督 rationale；dynamic token weighting 提高关键解释 token 的损失权重，从而在准确率与可解释性之间调节。作者还使用 verbalizer 将 up/down 映射到多个同义词，减少输出表面形式对训练的影响。

## Baselines / Comparisons

主实验在 ACL18 和 CIKM18 股票数据上比较 ARIMA、Adv-LSTM、StockNet、DTML，以及 zero-shot/微调 LLaMA-2、GPT-4 和 FinMA。指标为 Accuracy、F1 和 Matthews Correlation Coefficient；MCC 用于避免类别不平衡下只看 Accuracy 的误判。解释质量用 faithfulness 和 informativeness 评估，并用 GPT-4 判定事实能否由输入知识推断；500 个回答还由 3 名标注者多数投票核对，自动判定与人工标注 Pearson 相关为 0.826。

## Experiments / Findings

- ACL18 上 Ploutos-7B 达到 Accuracy 61.21、MCC 0.205；CIKM18 为 59.89/0.064。ACL18 的 ARIMA、Adv-LSTM、StockNet、DTML 分别为 51.42、57.24、58.23、57.44 Accuracy；GPT-4 为 53.08，LLaMA-2 为 52.74，FinMA 为 56.28。
- Ploutos 的 rationale faithfulness/informativeness 为 81.24/96.52，高于 FinMA 59.53/61.32、FinGPT 72.82/84.76 和 GPT-3 77.63/91.58。这个结果说明“能生成解释”和“解释有信息且可由输入支持”不是同一件事。
- 技术分析的 N2I-Align 在内部对照中达到 59.62 Accuracy、0.189 MCC，优于 Rand 50.68/0.002、LLMTime 51.12/0.028 和 LLMDate 53.47/0.082，显示提示结构对数值建模十分关键。

## Ablation / Error Analysis

ACL18 消融中，去掉 sentiment expert 的 Ploutos¬S 为 59.62/0.189/0.598，去掉 technical expert 的 Ploutos¬T 为 58.11/0.153/0.537，去掉 rearview-mirror 与 dynamic weighting 的 Ploutos¬R 为 60.41/0.191/0.604，完整模型才是 61.21/0.205/0.612。温度实验显示约 0.5 时 accuracy 与 faithfulness 同时较好，过低会过度强调涨跌标签，过高则削弱预测。文本-only 方案会偏向某一类别，数值-only 方案又缺少事件语义；二者都说明多专家融合的收益并非简单的模型规模收益。

## Limitations

数据使用历史 tweets/news 与价格，存在时间漂移、市场制度变化和潜在信息泄漏风险；GPT-4 既参与生成训练 rationale 又参与部分评价，可能引入 evaluator bias。解释 faithfulness 由可推断性近似，并不证明因果有效；论文也没有证明 rationale 真能改善交易收益或人的决策。

## 两句话总结

Ploutos 将情感、技术和基本面专家的异构信号交给金融 LLM，再用 rearview-mirror 数据与动态 token 加权训练可解释输出。它在两个股票数据集上同时改善涨跌预测和 rationale 指标，但结果仍受时序外推、评价器偏差和“可读理由不等于因果依据”的限制。

## Evidence note

本笔记逐段核对 PDF 的 Problem Formulation、Methodology、Tables 3-8、Ablation 与 Appendix evaluation protocol；正文数值按原表保留。

