# 489. GPT-NER: Named Entity Recognition via Large Language Models

- **Authors:** Shuhe Wang et al.
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2304.10428](https://arxiv.org/abs/2304.10428)
- **Tags:** `NER` `LLM-prompting` `self-verification` `hallucination`

## Introduction
NER 是 token-level sequence labeling，而 LLM 主要生成 text，导致零样本 NER 低于 supervised baseline，并且 LLM 会把 NULL input 过度标成实体。GPT-NER 将抽取任务改写成带特殊标记的生成，再加 self-verification 过滤幻觉实体。

## Method / Framework
例如 `Columbus is a city` 被改成生成 `@@Columbus## is a city`；position-aware format 让模型同时学习实体边界。自验证 prompt 让 LLM 对提取实体回答是否属于指定 entity type，再由 controller/concatenation strategy 组合多次候选，减少 null-input hallucination。

## Baselines / Comparisons
比较 supervised NER、few-shot LLM、MRC-NER、ChatGPT/LLM prompt 与 GPT-NER 的 flat/nested NER；数据覆盖五个常用 NER datasets，并测试 full-data 与 low-resource。

## Experiments / Findings
GPT-NER 将生成格式与 sequence labeling gap 对齐，在 flat NER 和 nested NER 上显著超过直接 prompting；self-verification 对 NULL input 尤其重要，避免模型过度输出实体。full-data 与 low-resource 结果都显示特殊 token、position/concatenation 和自检组合比单一 prompt 更稳健。

## Ablation / Error Analysis
去掉 self-verification、只输出 entity、去掉 position 或改变候选 concatenation 会增加 false positive 或边界错误；nested entity 的重叠边界更难。模型仍可能产生不存在实体、type confusion 或无法严格遵守标记格式，因此必须做格式解析和后处理。

## Limitations
生成式 NER 的成本高于 token classifier，prompt/特殊 token 和语言分布会影响结果；self-verification 让同一 LLM 二次判断，可能继承原判断偏差。高分不等于实体提取解释忠实，解释性主要来自可读 span 与自检过程。

## 两句话总结
GPT-NER 用特殊标记把 NER 变成 LLM 更擅长的文本生成，并用 self-verification 抑制空输入幻觉实体。它弥合了生成与序列标注的接口差距，但格式、边界和自检偏差仍需要结构化后处理。

## Evidence note
已读取任务重写、self-verification、flat/nested/full/low-resource 结果、controller/format ablation 和 error discussion。
