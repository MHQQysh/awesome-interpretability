# 485. StarCoder: May the Source Be With You!

- **Authors:** Raymond Li et al. / BigCode collaboration
- **Venue / year:** TMLR 2023
- **Paper:** [arXiv:2305.06161](https://arxiv.org/abs/2305.06161)
- **Tags:** `code-llm` `training-data` `evaluation` `responsible-ai`

## Introduction
代码 LLM 需要解释其训练数据、许可证和代码生成能力，才能用于软件工程。StarCoder 论文介绍透明数据治理、模型训练和代码 benchmark，间接为代码模型的行为审计与 provenance 分析提供基础。

## Method / Framework
模型基于 The Stack 去重代码数据，使用 15.5B 参数、8K context 和 fill-in-the-middle objective，并公开数据治理、训练/评估与 model card。实验覆盖 HumanEval、MultiPL-E、MBPP、DS-1000、Spider、code completion 和 infilling。

## Baselines / Comparisons
比较 CodeGen、CodeGeeX、InCoder、SantaCoder、GPT-NeoX 等开源/闭源代码模型，并按语言、completion/infilling、zero/few-shot 分层。数据许可、PII/代码过滤和训练样本 provenance 也作为 responsible baseline。

## Experiments / Findings
StarCoder 在多种代码生成与补全 benchmark 上达到强开源结果，尤其是支持多语言和 FIM；公开权重、数据过滤、model card 和 prompt/evaluation 让结果更可审计。论文还显示代码 LLM 仍可能生成有漏洞、许可证不兼容或训练数据近似复制的内容。

## Ablation / Error Analysis
不同语言、context length、FIM 与普通 causal completion 的差异说明训练目标改变行为；perplexity/benchmark 分数不能充分衡量代码安全与许可证合规。数据过滤、去重和 license policy 能减少风险，但会牺牲部分 coverage。

## Limitations
模型仍可能记忆训练代码，公开数据与真实企业代码分布不同；benchmark 主要测可执行性而不是解释忠实性、维护性和安全性。该论文不是 mechanistic interpretation，但提供了研究 code-model attribution/provenance 的重要基础。

## 两句话总结
StarCoder 通过透明数据、FIM 训练和多语言代码评测建立了可审计的开源代码 LLM。它提醒我们解释代码模型不仅要看 neuron/circuit，还要追踪数据来源、许可证、记忆复制和生成风险。

## Evidence note
已读取本地正文的模型/数据治理、训练目标、代码 benchmark、开源对照和 responsible-ai discussion。
