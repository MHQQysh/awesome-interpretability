# 168. On A Scale From 1 to 5: Quantifying Hallucination in Faithfulness Evaluation

- **Authors:** Xiaonan Jing, Srinivas Billa, Danny Godbout
- **Venue:** Findings of NAACL 2025
- **Paper:** https://aclanthology.org/2025.findings-naacl.433
- **Tags:** Hallucination, Faithfulness, LLM Judge, NLI

## Introduction

生产环境中的 guided NLG 不能只问“有没有 hallucination”，人工逐条核验成本高，二元 NLI 也无法表达部分支持、细节遗漏和严重程度。论文在 travel-domain industry data 上建立 1 到 5 的 faithfulness rubric，比较 LLM judge 与 NLI 模型，并估计部署 latency / cost。

## Method / Framework

将 grounding reference 与 generated hypothesis 对齐，用 rubric 从完全支持到严重不一致打分；同时生成 synthetic unfaithful data，覆盖 intrinsic contradiction 和 extrinsic unsupported content。比较 GPT-4 等 LLM judge、现成 NLI 和用合成数据微调的 NLI，另用 heuristics 估计 hallucination percentage。

## Baselines / Comparisons

比较 GPT-4 / 其它 LLM、TRUE 等 NLI、原始 NLI 与 synthetic-data fine-tuned NLI。指标包括与人工 / rubric 的 agreement、sensitivity、faithfulness score、latency 和调用成本。

## Experiments / Findings

- 在四个 travel-domain dataset 上，GPT-4 能较准确判断 source 与 generation 是否事实一致，并提供可读解释。
- 二元 NLI 能识别明显 contradiction，但对 partially supported 和长句细节不敏感；synthetic unfaithful data 微调可提升 NLI 的敏感性。
- 1 到 5 的分级比 binary label 更能反映生产系统中“足够可信”与“需要人工复核”的边界。
- 部署分析显示 LLM judge 的质量与成本 / latency 需要折中，分句或分 claim 的粒度会影响调用数和错误率。

## Ablation / Error Analysis

消融 sentence-level vs document-level、reference / hypothesis 分段、synthetic corruption 类型、LLM model 与 NLI fine-tuning。错误集中在隐式常识、数字和实体对齐、reference 未明确提及但模型补充合理知识的 extrinsic case。LLM judge 还可能受表述流畅性和 prompt rubric 影响。

## Limitations

travel domain 和合成错误不代表所有 NLG；rubric 分数仍含人工主观性。GPT-4 judge 的判断不是事实证明，NLI 训练数据和 evaluator 可能共享偏差。成本估计依赖 2025 API 价格与模型版本，部署前需重新测量。

## 两句话总结

论文用 1 到 5 的 rubric 将 faithfulness 从二元判断扩展为可量化严重程度，并显示 GPT-4 与合成数据增强 NLI 各有优势。它更适合生产中的风险分层，但领域迁移、隐式事实和 judge 成本仍是主要风险。
