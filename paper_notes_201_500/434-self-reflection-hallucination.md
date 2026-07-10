# 434. Towards Mitigating Hallucination in Large Language Models via Self-Reflection

- **Authors:** Ziwei Ji, Tiezheng Yu, Yan Xu, Nayeon Lee, Etsuko Ishii, Pascale Fung
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2310.06271
- **Tags:** `hallucination` `medical-QA` `self-reflection` `generate-score-refine`

## Introduction
本文从医学 generative QA 出发研究“听起来专业但事实错误”的 hallucination。医学问答既有罕见术语，又有错误答案可能造成现实伤害；单次生成的 fluent response 不能保证事实一致、与问题相关或被给定知识支持。作者因此把 LLM 的回答过程改成交互循环，而不是只依赖一次 prompt 或只用 n-gram 相似度评价。

## Method / Framework
方法包含三个循环。Factual Knowledge Acquiring Loop 先根据问题获取并生成候选背景知识，再用 factuality score 判断知识是否有足够证据；Knowledge-Consistent Answering Loop 根据知识生成回答，用 consistency score 检查回答是否与知识对齐；Question-Entailment Answering Loop 用 sentence-BERT 相似度检查回答是否真正蕴含/回应问题。低于阈值时，LLM 按指定 aspect 自我反思并 refine knowledge 或 response，直到达到阈值或达到最大循环次数。

这里的“self-reflection”不是让模型泛泛地说“请检查”，而是把分数和失败维度写进下一轮指令，例如告知 factuality 不足、或 response 与 knowledge 的一致性低，并要求只针对该维度重写。实验使用 Vicuna、Alpaca-LoRA、ChatGPT、MedAlpaca 和 Robin-medical，覆盖 PubMedQA、MedQuAD、MEDIQA2019、LiveMedQA2017、MASH-QA。

## Baselines / Comparisons
基线是五个模型直接 zero-shot 生成的回答，另有医学领域微调模型与通用模型的比较。指标包括 sample/sentence-level MedNLI、CTRL-Eval consistency、F1、ROUGE-L；人工评测把答案标为 query-inconsistent、tangential 或 entailed，并按句标为 fact-inconsistent、fact-consistent 或 generic。这样同时测事实、问题相关性和文本重叠，避免把“更像参考答案”误当成无幻觉。

## Experiments / Findings
self-reflection loop 在五个数据集、7B 与 175B 模型上都优于直接生成，MedNLI 尤其明显；论文指出 Alpaca-LoRA 在 PubMedQA 的 sample/sentence MedNLI 通过循环可获得约三倍于直接基线的提升。人工 PubMedQA 结果中，Vicuna 的 query inconsistency 从 0.67% 降到 0、tangentiality 从 6.04% 降到 2.00%、fact inconsistency 从 8.69% 降到 7.38%；ChatGPT 的 fact inconsistency 从 8.06% 降到 6.33%，tangentiality 由 18.00% 略降到 17.33%。

论文也观察到医学微调不必然更可靠：Robin-medical 在若干生成指标上明显较弱，而 instruction-tuned MedAlpaca 往往比非 instruction tuning 更适合这些 GQA 任务。循环能把“完全答非所问”和明显事实冲突改掉，但对 generic 或原本模糊的问题不一定带来同样幅度的收益。

## Ablation / Error Analysis
PubMedQA 消融显示三部分都重要。去掉 refinement 后 Vicuna 的 sample MedNLI 从 0.6380 降到 0.4520，ChatGPT 从 0.6824 降到 0.5180；不写明需要改善的 aspect 也会下降；不提供具体 score number 时，ChatGPT 的 sentence MedNLI 从 0.6598 降到 0.5989，CTRL-Eval 也变差。案例中，循环能够把“EtCO2 不可靠”改成与证据一致的正相关结论，也能去掉与问题无关的邮件式内容。

主要错误包括候选知识本身不可靠、阈值判断被 scorer 误导、模型在反思时引入新事实，以及复杂/含糊医学问题无法由一次检索或句向量相似度解决。人工评测的 Krippendorff alpha 对 query/tangentiality 超过 0.8、fact consistency 超过 0.7，说明标注信号可用但仍不是绝对金标准。

## Limitations
方法尚不能消除幻觉，复杂或含糊问题仍可能产生无依据内容；多轮知识获取、评分和重写带来额外延迟与 API 成本。医学数据主要是英文、任务和 scorer 也较受限，作者明确认为该方法在直接真实部署前仍应作为检索等方法的补充，而非独立安全保证。

## 两句话总结
本文用 factuality、knowledge-consistency 和 question-entailment 三个循环把 LLM 变成“生成—评分—反思—重写”的医学 QA 系统。实验证明显式反馈分数和失败维度能降低事实冲突、答非所问和切题性错误，但它仍受知识来源、评分器和多轮成本限制。

## Evidence note
已逐段读取本地 PDF，核对了五模型/五数据集设置、三循环方法、自动与人工评测、PubMedQA 人工结果以及 refinement/aspect/score-number 消融。
