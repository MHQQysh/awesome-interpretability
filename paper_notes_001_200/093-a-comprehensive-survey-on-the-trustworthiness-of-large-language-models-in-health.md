# 093. A Comprehensive Survey on the Trustworthiness of Large Language Models in Healthcare

> 人工精读笔记：Findings of EMNLP 2025 survey。本文不是单一方法实验，而是按医疗风险维度梳理数据、模型、任务、评估与缺口。

## 0. 论文信息

- **作者**：Manar Aljohani, Jun Hou, Sindhura V. S. S. R. K. 等
- **来源**：[Findings of EMNLP 2025](https://aclanthology.org/2025.findings-emnlp.356)
- **范围**：2022--2025 医疗领域 LLM trustworthiness 研究
- **维度**：truthfulness、privacy、safety、robustness、fairness/bias、explainability

## 1. Introduction：要解决什么问题

医疗 LLM 的错误不只是普通 benchmark 分数下降：幻觉、隐私泄露、偏见、不稳定和不可解释都可能直接影响临床决策、患者安全和医疗公平。现有工作分散在不同数据集、模型和 trustworthiness 维度，缺少统一视角来回答“评测了什么风险、用什么证据、离临床部署还有多远”。

作者通过系统检索和筛选近年研究，建立医疗 LLM 的任务/数据/模型图谱，并按六个 trustworthiness dimensions 组织方法。survey 的目标是暴露评测碎片化和临床验证不足，而不是给所有模型一个简单总排名。

## 2. Method / Framework：怎么组织综述

### 2.1 数据与任务

论文整理了临床问答、医学知识问答、诊断/决策支持、报告生成、摘要、患者沟通、代码/生物医学任务等数据；区分 web-scraped、医学考试、电子健康记录、指南/文献和合成数据。模型覆盖通用 decoder LLM、医疗专用模型、开放与闭源系统。

### 2.2 六个维度

- **Truthfulness**：事实正确、临床知识、幻觉与引用。
- **Privacy**：训练数据泄露、病人身份、去标识化和成员推断。
- **Safety**：有害建议、越权、拒答和临床风险。
- **Robustness**：分布外、对抗提示、拼写/格式、长上下文和数据变化。
- **Fairness/Bias**：性别、种族、年龄、语言和交叉群体差异。
- **Explainability**：证据引用、理由、可解释决策和医生可审计性。

## 3. Baseline / 对比方式

survey 中的 comparison 主要是研究协议层面：通用 LLM vs 医疗专用 LLM，闭源 vs 开源，zero-shot/few-shot vs instruction fine-tuning/RAG，以及不同 trustworthiness 维度的 benchmark。它也对比外部检测器、redaction/DP、adversarial stress test、retrieval grounding、multi-agent debate 和 mechanistic/interpretable methods。

因此不能把 survey 表格中的 accuracy 直接横向比较：数据、提示、医生评分、自动指标和临床任务往往不同，作者明确指出缺少标准化评价。

## 4. Findings：综述得到的主要结论

### 4.1 Truthfulness 与 hallucination

医疗模型即使在医学知识 benchmark 上表现高，也可能在开放式诊断/摘要中编造不存在的事实、引用或患者信息。RAG 和引用能够提高可验证性，但检索内容本身可能过时、冲突或不完整；因此“有 citation”不等于 clinical truthfulness。

### 4.2 Privacy

去标识化、差分隐私和 redaction 能降低泄露风险，但可能损害医疗文本的语义和临床效用。survey 强调需要同时测试 membership inference、prompt extraction 和真实攻击场景，而不是只检查数据中是否出现姓名。

### 4.3 Safety 与 robustness

医疗 LLM 面临危险建议、过度自信、拒答不足、提示注入和分布转移；更大的模型不自动解决这些问题。自动 stress test 和领域安全 benchmark 有进展，但临床真实 workflow 和医生在环验证仍少。

### 4.4 Fairness/bias

数据分布和社会偏差会反映在诊断建议、风险预测和患者沟通中。许多论文只测单一 demographic attribute，交叉群体、公平性与临床 outcome 的关系仍缺少统一协议。

### 4.5 Explainability

解释方法包括 attribution、attention、rationale、citation、检索证据和模型内部分析；但它们可能是 plausible 而非 faithful。survey 把“医生能读懂”与“理由真的驱动了模型”区分开，强调需要 causal/clinical validation。

## 5. Ablation / 机制解释

这篇 survey 的消融不是单模型组件消融，而是跨研究的协议对照：有无 retrieval grounding、有无 instruction fine-tuning、有无 privacy protection、有无 safety alignment，以及不同模型大小/开放程度。它揭示的规律是：一个维度的改进常以另一个维度为代价，例如隐私保护影响效用，强拒答影响可用性，简洁解释可能牺牲证据完整性。

## 6. Limitations / 局限与复现注意

- 文献检索截止时间和关键词会漏掉快速出现的新医疗模型。
- 不同研究的医生标注、临床任务和数据许可不同，无法合成一个统一 leaderboard。
- survey 汇总方法但不重新执行每个实验，原论文中的评估偏差会被继承。
- 医疗部署需要法规、责任、隐私和前瞻性临床试验，不能由离线 NLP 分数替代。

## 7. 两句话总结

这篇 survey 的核心问题是医疗 LLM 的 trustworthiness 不能由准确率单独代表，必须同时审计事实、隐私、安全、鲁棒、公平和解释性。它系统整理 2022--2025 的数据、模型和评估方法，指出当前最大缺口是标准化指标、交叉风险测试和真实临床验证不足。
