# 465. Against Opacity: Explainable AI and Large Language Models for Effective Digital Advertising

- **Authors:** Qi Yang, Marlo Ongpin, Sergey I. Nikolenko, Alfred Huang, Aleksandr Farseev
- **Venue / year:** ACM Multimedia 2023
- **Paper:** [DOI:10.1145/3581783.3612817](https://doi.org/10.1145/3581783.3612817)
- **Tags:** `explainable-ai` `digital-advertising` `LLM-interface` `CTR`

## Introduction
数字广告平台的 audience targeting、竞价与相关性评估高度不透明，营销人员面对大量竞品广告数据却难以形成可行动洞察。论文尝试把 CTR 预测/排序与 LLM 的内容解释结合，让营销人员理解“什么样的广告内容更有效”。

## Method / Framework
论文提出面向营销分析的 SoMonitor/SODA 系统：一端用机器学习对广告内容做 CTR prediction/ranking，另一端用 LLM 处理高表现竞品广告，抽取 target audience、customer needs、product features 等 content pillars，再以 explainable-AI 界面帮助人机协作。公开材料表明它的重点是系统框架和工作流，而非一个新的基础模型。

## Baselines / Comparisons
公开可访问版本没有提供完整可核对的 benchmark 表格；系统定位上对照传统广告分析/人工竞品阅读、黑盒 CTR 模型与只使用 LLM 的文本总结。不能把摘要中的“improve transparency”解释成已证明的 causal uplift。

## Experiments / Findings
可获得摘要与元数据支持的 finding 是：将 CTR scoring、竞品内容语义抽取和 LLM 对话解释放在一起，可以把大规模广告数据转成受众、需求和产品特征层面的可读摘要。公开材料未给出足以逐项复核的样本规模、CTR 数值或用户实验，因此本文档不虚构具体提升百分比。

## Ablation / Error Analysis
完整 ablation、用户研究和错误案例在当前公开访问范围内不可核对。系统风险包括 CTR 预测偏差被 LLM 解释包装、竞品内容总结的选择偏差，以及“可读说明”与实际广告因果机制不一致。

## Limitations
该 ACM Multimedia 条目为闭源出版商全文，当前本地没有可读 PDF；本笔记严格区分了摘要/元数据证据与未能验证的细节。营销领域的 explanation 需要对数据漂移、平台策略、隐私和广告公平性做额外审计。

## 两句话总结
论文把 CTR 预测和 LLM 语义总结组合成营销人员可用的 explainable advertising workflow。它的价值在于降低数据解释门槛，但公开可核对证据不足以证明预测提升或解释忠实性，不能把产品愿景当成机制验证。

## Evidence note
全文访问受出版商限制；已核对 DOI/ACM 元数据和公开摘要，并明确记录证据边界，而不是补写未读实验数字。
