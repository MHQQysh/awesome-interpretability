# 376. Interpretable Online Log Analysis Using Large Language Models with Prompt Strategies

- **Authors:** Yilun Liu, Shimin Tao, Weibin Meng, Jingyu Wang, Wenbing Ma, Yuhang Chen, Yanqing Zhao, Hao Yang, et al.
- **Venue / year:** ICPC, 2024
- **Paper:** https://arxiv.org/abs/2308.07610
- **Tags:** `LLM` `log-analysis` `prompt-engineering` `CoT` `interpretability` `anomaly-detection`

## Introduction
日志分析是软件维护和运维中的典型解释性任务：系统不只需要判断一条日志是否异常，还需要解析模板、说明关键字段、帮助工程师定位根因。传统 supervised parser/anomaly detector 通常依赖目标域标注数据，换系统后性能下降；而只让 ChatGPT 输出一个标签又无法给出稳定、可读的分析。

LogPrompt 的问题设定是 online/zero-shot log analysis：不使用目标域 in-domain training，能否用 prompt strategy 激活 LLM 已有的语言和推理能力，同时输出人可以检查的 interpretation。论文覆盖 log parsing 和 anomaly detection 两类任务，并特别关注新域、非自然语言 token、日志短而信息密集时的失败。

## Method / Framework
LogPrompt 不是微调模型，而是一个 prompt 组合：

- **Self-prompt / in-context examples:** 给出少量带标签或解析示例，诱导模型从输入格式和标签对应关系中形成任务上下文。
- **Implicit CoT:** 要求模型给出判断理由；**Explicit CoT:** 再把分析拆成中间步骤和最终答案格式。日志异常检测使用 CoT 的逻辑链，日志解析更多使用 self-prompt，因为 parsing 本身相对容易。
- **Format constraints:** 约束输出字段、模板或 anomaly label，减少自由生成导致的无法评估和不可读结果。
- **Interpretation:** 让 LLM 同时解释字段含义、异常原因或判定依据，使结果成为工程师可读的报告，而不是黑箱分数。

实验用 ChatGPT 175B，另用 Vicuna 13B 验证开源和小模型兼容性。online setting 下每个域只提供 prompt 中的少量示例，不进行 in-domain 参数更新。

## Baselines / Comparisons
比较包括 simple prompt 的 ChatGPT、传统/训练型日志解析与异常检测方法，以及 LogPPT、LogStamp 等 LLM-based 方法。任务指标包括 parsing 的 F1、anomaly detection 的 session-level F1 (S-F1)、template-level F1 (T-F1)、precision/recall；解释质量由 6 位平均拥有十年以上经验的日志分析人员从 usefulness 和 readability 评分。

基线公平性要注意：LogPrompt 的主要增益来自输入示例、理由和格式，而不是新的模型权重；因此它回答的是“如何调用 LLM”而不是“训练出更好的 detector”。

## Experiments / Findings
论文在 9 个公开数据集上测试解析和异常检测。总体结论是 prompt 让没有 in-domain training 的 LLM 具备跨域能力：摘要报告相对 simple prompt 最高提升 380.7%，相对训练方法最高提升 55.9%。对 Vicuna 13B，LogPrompt 在 HDFS、Linux、Proxifier 等数据集的 parsing 表现可接近 175B ChatGPT；相对 Vicuna simple prompt，平均 F1 提升 380.7%。

解释人工评测中，log parsing 的 usefulness/readability 平均分为 4.23/4.60，anomaly detection 为 4.30/4.54；对应高于 4 分的样本比例大多超过 80%。实践者认为解释可以帮助快速理解陌生第三方服务日志、写事故报告和判断 false alarm 是否可信。

## Ablation / Error Analysis
CoT 消融在 BGL 上非常直观：ChatGPT simple prompt 的 S-F1/T-F1 为 0.189/0.115；加入 implicit CoT 后为 0.307/0.183；explicit CoT 后为 0.384/0.321。Spirit 上则为 0.445/0.086、0.443/0.096、0.450/0.116，说明 CoT 的收益与数据集和异常定义有关，不能简单认为越长越好。

in-context 示例数量也有倒 U 形：BGL 上大约 20 个 labelled logs 时 F1 和 precision 达峰，继续增加会下降；过长上下文可能让模型把注意力放在示例格式而不是新日志。低评分案例主要来自领域知识不足、把专用参数概括成普通 object，以及日志包含大量数字、地址和代码而缺少自然语言语义。论文还发现 BGL 有“语义正常却被标为异常”的公开标签噪声，影响绝对分数。

## Limitations
LLM 仍是黑箱，训练语料可能已经包含某些日志片段；zero-shot 并不等于没有数据泄漏。Anomaly detection 在没有域训练时仍然困难，ChatGPT 的最佳 S-F1 只有约 0.450。prompt 依赖模型版本、示例数量和格式，API 成本与隐私也限制工业部署；Vicuna 结果说明可行性，但不证明所有小模型都能复现。

## 两句话总结
LogPrompt 用 self-prompt、少量 in-context examples、CoT 和格式约束，把通用 LLM 变成无需目标域微调的日志解析与异常分析器，并要求输出可读理由。它的消融证明“要求说明理由”和适量示例能显著提高异常检测，但领域知识、日志非自然语言字段、标签噪声和上下文过长仍是主要失败来源。

## Evidence note
已读取 arXiv 2308.07610 本地 PDF 文本；数值来自人工解释表、BGL/Spirit CoT 消融、Vicuna 对比和 prompt 数量实验。该论文与清单中较早的 LogPrompt 条目存在同名/近重复，应在最终 README 去重时保留版本关系。
