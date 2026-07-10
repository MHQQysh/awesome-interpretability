# 377. Empowering Time Series Analysis with Large Language Models: A Survey

- **Authors:** Yushan Jiang, Zijie Pan, Xikun Zhang, Sahil Garg, Anderson Schneider, Yuriy Nevmyvaka, Dongjin Song
- **Venue / year:** IJCAI, 2024
- **Paper:** https://arxiv.org/abs/2402.15391
- **Tags:** `survey` `time-series` `LLM` `tokenization` `interpretability`

## Introduction
时间序列模型长期在预测、分类、异常检测和补全上使用专门架构，但不同数据集、领域和任务往往需要重新设计模型。LLM 的预训练知识、in-context learning 和自然语言接口提供了另一条路线：把数值序列转成语言模型可处理的 token，或让 LLM 与时间序列 encoder/decoder 结合。

这篇 survey 的解释性价值在于整理了“数值信号如何进入 LLM、LLM 的语言知识如何返回时间序列任务”的完整路径。它同时提醒：LLM 的文本解释未必反映预测依据，时间序列 tokenization、跨模态对齐和 evaluation protocol 都会影响所谓的 reasoning。

## Method / Framework
论文按模型接入方式构建 taxonomy：

1. **Direct query / prompting:** 直接将序列统计、趋势或离散描述放入 prompt，利用 LLM 的 zero/few-shot 能力。
2. **Time-series tokenization:** 将连续值、patch、频域信息、趋势/季节性或统计摘要映射到离散 token/embedding，再接入 LLM。
3. **Architecture integration:** 使用 frozen LLM + projection、cross-attention、encoder-decoder 或把时间序列模型作为前端，让文本和数值表示交互。
4. **Fine-tuning:** 讨论 full fine-tuning、parameter-efficient tuning、instruction tuning、对齐多任务和 domain adaptation。
5. **LLM-enhanced time-series models:** LLM 不一定直接输出预测，也可以生成先验、解释、检索信息、变量关系或作为 planner/agent。

应用章节覆盖 forecasting、classification、anomaly detection、imputation、event/causal analysis、health/finance/traffic 等。解释方法则包括自然语言理由、趋势/周期描述、变量重要性、检索证据和将内部表示 probe 出来。

## Baselines / Comparisons
作为 survey，论文把传统 statistical models、RNN/CNN/Transformer/专用 time-series foundation models 与 LLM-based models 放在同一发展线上。比较维度不只是 MAE/MSE：还包括 tokenization 代价、上下文长度、跨域泛化、few-shot 能力、计算成本和可解释性。

它特别指出，直接 prompt 的 baseline 往往更容易展示语言解释，却未必在数值预测上公平；而 frozen LLM + learned projector 可能提高任务分数，却把解释性变成 projection 和 decoder 的黑箱。因此需要分别报告预测质量、表示对齐和解释 faithful 性。

## Experiments / Findings
论文没有统一重跑所有方法，而是做系统综述。其发展流程是：先用自然语言描述序列做 direct query；随后出现数值 tokenization 和 patch 化，把长序列压进 LLM 上下文；再通过 projector、cross-attention 和 instruction tuning 建立时序-语言对齐；当前方向转向 time-series foundation model、检索增强、多模态外部知识和 agentic analysis。

综述得到的主要判断是：LLM 的通用知识和 in-context reasoning 对少样本、跨任务接口有吸引力，但时间序列的连续性、长上下文、非平稳性和数值精度不能靠语言 token 自动解决。性能提升经常来自额外的时间序列 encoder、任务专门训练或数据规模，因此不能把所有收益归因于 LLM 的“推理”。

## Ablation / Error Analysis
论文总结的关键消融包括 token 粒度、patch 长度、是否保留时间戳/变量名、frozen vs tuned LLM、projector 结构、是否加入 trend/seasonal decomposition、不同 prompt 和不同 context length。过粗 token 会丢失峰值和异常，过细 token 会造成上下文爆炸；只看语言化趋势会忽略跨变量依赖和相位关系。

解释错误主要有三类：模型给出与预测不一致的事后理由；文本表述看似合理但没有对应数值证据；模型把训练语料中的常识当成当前序列的因果依据。survey 因此建议把 explanation evaluation 与 intervention、feature ablation、counterfactual forecasting 和 calibrated uncertainty 结合，而不是只用人工流畅度评分。

## Limitations
这是跨领域 survey，所覆盖方法的实验协议、数据集和指标不统一，不能从综述表直接推出一个 universal winner。很多论文使用闭源 LLM，无法检查内部表示；时间序列解释也缺少统一 ground truth。未来模型可能改变 tokenization 和上下文限制，当前 taxonomy 需要持续更新。

## 两句话总结
这篇 survey 将 LLM 用于时间序列的路线整理为 direct prompting、数值 tokenization、架构融合、fine-tuning 和 LLM-enhanced models，并串起预测、异常和解释等应用。它最重要的解释性提醒是：自然语言 rationale 只是输出层行为，只有结合数值扰动、表示 probe 和反事实实验，才能判断解释是否真的来自时间序列证据。

## Evidence note
已读取 IJCAI 2024 survey 的本地 PDF 文本和 taxonomy 章节；本文没有统一实验数字，结论均按 survey 的方法分类和未来方向归纳。
