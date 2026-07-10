# 034. Angry Men, Sad Women: Large Language Models Reflect Gendered Stereotypes in Emotion Attribution

- **作者 / venue**：Flor Miriam Plaza-del-Arco, Amanda Cercas Curry, et al.；ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.415/)
- **任务**：性别化 persona 下的 emotion attribution bias

## 1. Introduction

LLM 会反映训练语料中的社会规范和 stereotype，但 emotion analysis 中的性别偏见研究相对不足。论文的问题不是模型能否识别情绪，而是当同一个事件分别由“男人”和“女人”经历时，模型是否会系统地给出不同情绪，并把 stereotype 当作个体心理推断。

## 2. Method

- 使用 ISEAR emotion antecedent/reaction 数据。
- 对每个事件构造 man/woman persona，并使用三种 persona prompt template。
- 五个 state-of-the-art LLM，包含 Llama2-7B/13B/70B、Mistral-7B-Instruct 和 GPT-4。
- 每个事件和 persona 多次生成，过滤多词/无关回答，统计 emotion distribution。
- 对生成理由做定性分析，观察模型如何解释性别与情绪的关联。

## 3. Baseline 与对比

- 同一事件的 male vs female persona 是核心 counterfactual baseline。
- 三种 prompt template 检查 stereotype 是否由特定措辞造成。
- 五个模型之间比较规模、开放/闭源和 instruction tuning 的影响。
- 与心理学/性别研究中的 stereotype 规律对照，而不是只用模型内部自定义指标。

## 4. Findings

- 五个模型都表现出稳定的 gendered emotion attribution；论文特别观察到女性更容易被关联到 SADNESS，男性更容易被关联到 ANGER 等 stereotype。
- 这种偏差在不同 prompt template 和模型规模下仍出现，说明不是单一 prompt artifact。
- 模型生成的解释常把社会 stereotype 包装成自然的心理推断，导致用户误以为结果反映真实个体差异。
- 论文的意义不是证明某个模型“有偏见”一次，而是展示 emotion application 中 persona-conditioned generation 会系统性重现社会刻板印象。

## 5. 局限与风险

使用二元性别 persona 简化了真实身份；ISEAR 的文化和时代分布也会影响结果。emotion attribution 不能直接当作心理诊断或个体预测，部署时必须做 subgroup audit 和安全限制。

**一句话评价**：该研究把 LLM bias evaluation 从职业/性别词关联扩展到“同一事件的情绪归因差异”，证明解释文本本身也可能传播 stereotype。
