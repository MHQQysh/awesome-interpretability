# 269. Mechanistic Interpretability of Large Language Models with Applications to the Financial Services Industry

- **Authors:** Ashkan Golgoon, Khashayar Filom, Arjun Ravi Kannan
- **Venue / year:** ACM 2024
- **Paper:** https://arxiv.org/abs/2407.11215
- **Tags:** Mechanistic Interpretability, Finance, Fair Lending, Direct Logit Attribution, Activation Patching

## Introduction

金融机构需要对模型偏见、公平性、合规性和可靠性负责，但 GPT 类模型内部决策过程不透明。论文以 fair-lending compliance monitoring 为具体案例，不追求一个金融分类器的 SOTA，而是示范如何用 transformer circuits 工具定位“模型为什么回答 Yes/No”的组件。

## Method / Framework

研究使用 GPT-2 Small，构造 clean/corrupted prompts，让模型判断某个贷款场景是否可能违反 Fair Lending law。先用 final-token logit difference 与 direct logit attribution 分解各层 residual、MLP 和 attention 对 Yes/No 的贡献；再做 attention-head attribution 和 activation patching，把 clean prompt 的 activation 注入 corrupted prompt，定位具有因果影响的 heads。论文还回顾 transformer、attention、MLP、unembedding 和 mechanistic interpretability 基础，适合金融合规读者理解工具链。

## Baselines / Comparisons

这篇工作是 case study，不提供传统分类 baseline 或大规模金融 benchmark。比较对象是 IOI 等经典 circuit localization 的预期，以及 layer-by-layer logit lens、attention head decomposition、clean/corrupted activation patching 三种分析视角。评价重点是 logit difference/probability ratio 和组件定位，而不是 accuracy/F1。

## Experiments / Findings

- Fair-lending prompt 的平均 Yes/No logit difference 为 0.81，normalized probability ratio 为 2.26，说明模型输出虽有偏好，但不是极端确定。
- logit difference 通常随层数逐步改善，不像 IOI 可只定位少数末层；MLP layer 2 和 4 对任务完成有明显正贡献，MLP layer 10 与 attention layer 0 反而降低表现，说明该合规任务同时依赖 attention 与 MLP。
- attention head 分解发现正向贡献头包括 11.4、8.9、6.3，负向贡献头包括 0.6、11.0、10.7；摘要级 clean/corrupted patching 进一步报告正向 heads 10.2、10.7、11.3 与负向 heads 9.6、10.6 重要。两组编号对应不同 attribution/patching视角，不应简单合并成唯一电路。

## Ablation / Error Analysis

layer attribution 只看最终 logit，可能把早层信息传递和后层读出混在一起；activation patching 才能更接近“必要组件”，但 clean/corrupted prompt 的选择会改变结果。金融规则本身需要法律专家定义，模型对“potential violation”的回答不等于真正合规判断；小样本示例也无法评估偏见的统计稳定性。

## Limitations

只研究 GPT-2 Small 和一个人工设计的 Fair Lending 任务，不能直接外推到 GPT-4、金融 LLM 或真实贷款审批。作者展示了 interpretability workflow，不是经过法律验证的 compliance system；heads 的编号也依赖模型 checkpoint 与 prompt。后续需要真实分布数据、群体公平性指标、专家审计和跨模型复现。

## 两句话总结

论文把 direct logit attribution、layer/head decomposition 和 activation patching 应用于 Fair Lending 合规提示，展示了金融场景如何从答案追到具体 transformer components。它的价值是提供可复用的机制分析范式，而不是证明 GPT-2 的金融判断正确或某组 heads 在所有模型中通用。

## Evidence note

本笔记逐段核对 ACM/arXiv PDF 的背景、Fair Lending task、Tables 2-3、layer/head attribution 与 activation patching；同时保留摘要与 patching 结果的编号差异。

