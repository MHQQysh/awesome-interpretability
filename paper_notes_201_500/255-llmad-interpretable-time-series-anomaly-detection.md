# 255. Large Language Models Can Deliver Accurate and Interpretable Time Series Anomaly Detection

- **Authors:** Jun Liu, Chaoyun Zhang, Jiaxu Qian, Minghua Ma, Si Qin, Chetan Bansal, Qingwei Lin, Saravan Rajmohan, Dongmei Zhang
- **Venue / year:** 2024 arXiv; ACM publication record 2025
- **Paper:** https://arxiv.org/abs/2405.15370
- **Tags:** Time Series Anomaly Detection, LLM, In-Context Learning, AnoCoT

## Introduction

传统 TSAD 模型能给出异常标签，却很难说明为什么某个点或连续区间异常；同时，窗口长度、异常类型和数据集分布差异使 zero-shot LLM 很难直接处理数值序列。LLMAD 的目标是在不训练一个专用黑盒 detector 的情况下，用 LLM 同时完成检测、异常类型识别、紧急程度判断和可读解释。

## Method / Framework

LLMAD 将每个测试窗口与历史数据中的相似正常/异常片段进行动态检索，把正负 examples 放进 few-shot prompt。模型随后用 Anomaly Detection Chain-of-Thought (AnoCoT) 按专家式流程分析：global 检查整体范围，local 定位偏离点/区间，recheck 再验证是否符合数据集规则，并输出异常类型、报警等级和解释。与把预测误差当异常信号的 LLMTIME 不同，LLMAD 直接让 GPT-4 在上下文中执行分类和解释。

## Baselines / Comparisons

实验使用 KPI、Yahoo、WSD 三个单变量数据集，比较 SPOT、SRCNN、DONUT、VQRAE、AnoTransfer、Informer、TFAD、Anomaly Transformer 和 LLMTIME。指标是 Best F1 与 Delayed F1：前者衡量阈值调优后的峰值检测能力，后者惩罚超过允许延迟才发现连续异常的情况。解释由 5 名有平均 3 年 TSAD 经验的 DevOps 工程师评价 usefulness/readability；异常类型还与 Llama-3-70B、GPT-3.5 和 GPT-4 对比。

## Experiments / Findings

- 三数据集平均 Best F1/Delayed F1 为 0.759/0.588；KPI 为 0.843/0.667，WSD 为 0.711/0.401，Yahoo 为 0.724/0.695。LLMAD 的平均 Best F1 最高，Delayed F1 接近最优，但 Anomaly Transformer 在某些数据集的 Best F1 更高。
- 动态检索优于固定 examples；使用 2 个 positive + 1 个 negative 是效果与上下文开销的较好折中。AnoCoT 相对普通 CoT 将平均 Best F1 再提高约 6.2%，普通 CoT 相对无 CoT 提高约 9.5%。
- 人工解释评分中，AnoCoT usefulness 平均 4.06、readability 2.66，标准 CoT 为 3.58/2.57；AnoCoT 的 usefulness/readability high-in-percentage 分别为 72.37%/97.11%。GPT-4 的异常类型 Acc 为 WSD 0.900、KPI 0.790、Yahoo 0.930。

## Ablation / Error Analysis

无 ICL、固定检索、动态检索构成了逐级对照；更多 examples 在超过 2 positive/1 negative 后收益趋于平缓。无 rule/domain knowledge 时 WSD Best F1 从 0.716 降至 0.423，KPI 从 0.843 降至 0.652，说明 AnoCoT 的规则注入而非文字变长是主要收益。KPI 的长持续异常可能跨越窗口，Yahoo 的标注有时只标注 level shift 的起点，导致 LLMAD 把后续持续值也判为异常，出现 precision 下降。

## Limitations

LLMAD 依赖 GPT-4 级别的 instruction following，Llama-3-70B/GPT-3.5 明显更弱；动态检索、长窗口和 API 调用也带来成本。400 点窗口的 AnoCoT 平均总时延 17.03 秒、每日估算成本约 0.18 美元；这对监控尚可，但不等同于所有实时系统都可接受。解释质量主要是专家主观评分，且异常 ground truth 本身不包含解释标签。

## 两句话总结

LLMAD 用相似正负样本做 ICL，再用包含 global/local/recheck 的 AnoCoT 让 LLM 直接检测异常并生成工程师可读的理由。它在三类 TSAD 数据上取得有竞争力的 F1，并显示领域规则和动态示例比普通 CoT 更关键，但窗口、标签、模型能力和 API 成本限制了泛化。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、Method、Tables 1-8、human evaluation、case study 与 Conclusion；正文数值按原表保留。

