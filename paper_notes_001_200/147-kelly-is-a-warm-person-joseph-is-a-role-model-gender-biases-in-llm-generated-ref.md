# 147. “Kelly is a Warm Person, Joseph is a Role Model”: Gender Biases in LLM-Generated Reference Letters

- **Authors:** Yixin Wan, George Pu, Jiao Sun, Aparna Garimella, Kai-Wei Chang, Nanyun Peng
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.243
- **Tags:** Gender Bias, Fairness, Reference Letters, Hallucination

## Introduction

LLM 生成推荐信会直接进入求职、升学等专业场景；如果模型把男性写成 agentic、leader，把女性写成 communal、well-liked member，就可能影响真实机会。已有 bias benchmark 多测词级或通用 NLG，缺少针对 recommendation letter 这种高风险专业文档的系统分析，也较少区分输入提供的偏差与模型自身放大的偏差。

## Method / Framework

作者设计两种生成场景：Context-Less Generation（CLG），只给候选人姓名和简单描述，用于测模型内在偏见；Context-Based Generation（CBG），再提供个人信息和经历，用于测输入内容与模型偏见的交互。

偏见分三类：lexical content（名词和形容词选择）、language style（formality、positivity、agency 等风格）和 hallucination bias，即模型在没有输入支持时生成的内容进一步放大性别差异。作者用 Odds Ratio 比较男女候选人的词项概率，并用 NLI / entailment 检测 hallucinated statements。

## Baselines / Comparisons

主要比较 ChatGPT 与 Alpaca，在常见男性 / 女性姓名、相同经历和不同上下文条件下生成推荐信。baseline 不是单一模型，而是 gender-swapped prompt、CLG 与 CBG 对照，以及社会科学文献定义的 professionalism、excellency、agency、communal language 指标。

## Experiments / Findings

- 两个 LLM 都表现出显著性别偏差：男性候选人更常被描述为 agentic、natural leader、assertive；女性更常被描述为 warm、helpful、well-liked、supportive。
- CLG 能揭示模型在缺少候选人信息时的内在 stereotype；CBG 虽然加入经历，仍可能在没有证据时补充性别化人格、家庭或社交内容。
- 词汇偏差和语言风格偏差并不完全相同：某些信件词频差异不大，但 professionalism、positivity 或 agency 的整体风格仍明显不同。
- hallucination bias 使风险更严重：模型不是简单复述输入，而是在生成的无根据内容中继续强化 stereotype。
- ChatGPT 与 Alpaca 的偏差模式不同，说明只看一个模型或只看平均词频会漏掉部署相关的公平风险。

## Ablation / Error Analysis

gender-swapped names、context-less / context-based、名词 / 形容词 / style dimensions 和 NLI hallucination 检测构成主要消融。错误分析显示，控制候选人经历可以降低部分差异，却不能消除模型从名字、职业和社会语境推断性别的倾向；NLI 还会把模糊但合理的推荐信句子判成 unsupported，因此需要人工语义审查。

## Limitations

名字是性别和文化的粗糙代理，二元 gender 设置不能代表非二元、跨文化或姓名多义性。推荐信质量本身难以用自动指标定义，Odds Ratio 只能展示差异而不等于因果歧视。研究关注两个 2023 年模型和英文模板，不能直接推出其它模型、语言或真实招聘结果。

## 两句话总结

论文把性别公平分析带入 LLM 生成推荐信，显示模型会在 lexical content、语言风格和 hallucinated content 三个层面放大男性 agentic、女性 communal 的 stereotype。结果说明专业文档不能只检查表面流畅性，必须对无证据生成和 downstream allocational harm 做专门审计。
