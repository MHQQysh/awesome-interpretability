# 483. Gender Bias and Stereotypes in Large Language Models

- **Authors:** Hadas Kotek, Rikker Dockum, David Q. Sun
- **Venue / year:** ACM Collective Intelligence Conference 2023
- **Paper:** [DOI:10.1145/3582269.3615599](https://doi.org/10.1145/3582269.3615599)
- **Tags:** `bias` `gender-stereotype` `self-explanation` `faithfulness`

## Introduction
论文研究 LLM 是否把职业与性别的社会刻板印象错误绑定，并特别关注模型在句法歧义下是否忽略证据。另一个解释性问题是：模型给出的理由是实际依据，还是对偏见答案的 post-hoc rationalization。

## Method / Framework
作者设计一个不同于可能已进入训练语料的 WinoBias 的 occupation paradigm，测试四个近期 LLM 对男性/女性人物和职业的选择；再构造语法歧义句，比较自然 prompting 与显式要求模型识别 ambiguity 的结果，并人工核查模型 explanations。

## Baselines / Comparisons
比较模型选择与官方职业统计 ground truth、公众 stereotype perception，以及被明确提示“考虑歧义”前后的输出。WinoBias/传统 bias benchmark 作为相关背景，而非唯一评测集。

## Experiments / Findings
模型选择 stereotype-aligned occupation 的概率约为反向选择的 3--6 倍，且更接近人类刻板印象而非官方 job statistics；模型还会把 bias 放大到超过 perception/ground truth 的程度。研究样本中 95% 的关键句法歧义被模型忽略，但显式 prompt 后能识别；生成解释常事实不准确，可能遮蔽真正的偏见机制。

## Ablation / Error Analysis
“显式提醒 ambiguity”是最关键的行为对照：模型具备识别能力却不会默认调用。错误解释不能被当作透明证据，尤其当解释把 stereotype 说成语法/常识时，用户可能误以为模型进行了中立推理。

## Limitations
四个模型、英语职业句式和小规模人工评估不能覆盖所有性别/语言/身份群体；选择概率受 prompt、decoding 和模型版本影响。行为偏见分析也不能单独定位内部 circuit。

## 两句话总结
论文发现 LLM 对职业性别的 stereotype 选择可比反向选择高 3--6 倍，并且会忽略 95% 的句法歧义。更危险的是模型会为偏见生成不准确的理由，因此自我解释可能是合理化而不是透明化。

## Evidence note
已核对公开全文/开放版本的 occupation paradigm、四模型比较、ambiguity prompt 对照和 explanation finding。
