# 202. Efficient Toxic Content Detection by Bootstrapping and Distilling Large Language Models

- **Authors:** Jiang Zhang, Qiong Wu, Yiming Xu, Cheng Cao, Zheng Du, Konstantinos Psounis
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/30178
- **Tags:** Toxicity Detection, Rationale Distillation, Decision Tree of Thought, Transfer

## Introduction

有害内容标注成本高、边界依赖语境，任务微调模型又容易过拟合某一个数据集并只能输出二元标签。LLM 有 zero/few-shot 泛化和解释能力，却需要复杂 prompt，直接在线调用成本高；论文要同时获得 LLM 的 rationale 和小模型的效率、迁移性。

## Method / Framework

作者提出 Decision-Tree-of-Thought (DToT)：当 LLM 对初始毒性判断置信度低时，自动选择更细粒度的上下文/分析视角重新提问，逐步形成决策树并抽取更可靠的 label-rationale。随后把 DToT 生成的 rationale 与标签一起蒸馏给较小的 Flan-T5 student，使 student 同时预测 toxicity 和理由，避免部署时重复请求 teacher LLM。

## Baselines / Comparisons

实验覆盖 Toxigen、SBIC、DHate 和一个 Amazon 数据集；比较 RoBERTa fine-tuning、FC-T5、ChatGPT 的 CoT、DToT、DToT+few-shot、DToT+reasoning，以及只用标签和同时用标签/rationale 的 student。指标包括 accuracy、F1、AUC，并关注跨数据集 transfer，而非只看单一训练集。

## Experiments / Findings

- DToT 通常优于直接 CoT，尤其在 LLM 低置信或隐式毒性场景，因为它会继续询问更具体的视角而不是过早下结论；ChatGPT 的 DToT 在 Toxigen 上表中达到 85.03 accuracy、82.76 F1。
- 3B FC-T5 以 DToT rationale 微调后，在 SBIC 和 DHate 上可以超过 teacher LLM，说明结构化 rationale 能把广泛知识压缩进更便宜的任务模型，并提高跨数据集迁移。
- student 同时使用 label 与 rationale 比只用 label 更稳定；不同规模 Flan-T5 都受益，表明收益不是单纯来自扩大参数量。

## Ablation / Error Analysis

消融 DToT 深度、few-shot 示例、rationale loss、student size 和 teacher/student 组合。只做 CoT 会遗漏隐式冒犯，树搜索过深则增加提示成本；rationale 本身若包含错误或过度安全化判断，会把偏差蒸馏进 student。隐式毒性、讽刺、群体语境和新型表达仍是主要错误来源。

## Limitations

DToT 依赖 teacher 置信度和 prompt 视角，生成 rationale 的成本虽比人工标注低但不为零；自动解释也可能看似充分却并未真实参与判别。毒性标签具有文化和平台依赖性，四个数据集不能覆盖所有语言、社区规范和 multimodal content。

## 两句话总结

DToT 通过低置信度时递归细化问题，让 LLM 生成更可靠的毒性判断与 rationale，再把两者蒸馏给高效小模型。结果显示 rationale distillation 能增强跨数据集迁移，但 teacher 偏差、隐式毒性和解释忠实度仍需要人工与安全评测。
