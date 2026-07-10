# 137. Rethinking the Evaluation for Conversational Recommendation in the Era of Large Language Models

- **Authors:** Xiaolei Wang, Xinyu Tang, Wayne Xin Zhao, Jingyuan Wang, Ji-Rong Wen
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.621
- **Tags:** Conversational Recommendation, Evaluation, User Simulation, Explainability

## Introduction

传统 conversational recommender system（CRS）在固定对话上把预测 item 与人工标注 ground-truth 匹配，常用 recall、precision 或 item hit。作者发现这套协议不适合 ChatGPT：数据对话通常是含糊的 chit-chat，用户偏好未必明确；系统如果先追问澄清，在固定数据里反而被判错；而真实 CRS 的价值正是通过多轮互动逐步获取偏好。

论文先测 ChatGPT 的推荐能力，再解释它为什么在传统协议下表现一般，最后提出 iEvaLM：用 LLM 模拟用户与 CRS 多轮互动，并同时评估推荐准确性、自然度、实用性和解释的说服力。

## Method / Framework

iEvaLM 使用已有 ReDial 和 OpenDialKG 的 ground-truth item 构造 simulated user persona，再用 text-davinci-003 按手工行为规则模拟用户。交互最多 5 轮，temperature=0。每轮用户可以提供偏好、评价推荐或完成对话。

有两种交互形式：

1. **Attribute-based QA：** 系统只能询问预定义属性或推荐，模拟用户按目标 item 回答。
2. **Free-form chit-chat：** 系统和用户都可以主动澄清、聊天、推荐和反馈，更接近真实对话。

评估使用每轮 recommendation action 的 recall，以及最终推荐解释的 persuasiveness（0 到 2）。作者还验证用户模拟器的 naturalness / usefulness，并用 LLM scorer 减少大规模人工评测成本。

## Baselines / Comparisons

数据集是 ReDial（10,006 dialogues，182,150 utterances，电影）和 OpenDialKG（13,802 dialogues，91,209 utterances，电影、图书、体育、音乐）。基线包括传统 CRS 模型、zero-shot ChatGPT、ChatGPT 加外部 recommendation model，以及用户模拟器方面的 DialoGPT 和真实数据 utterance。论文把传统 fixed-conversation accuracy 与 iEvaLM 的互动 recall / persuasiveness 对比。

## Experiments / Findings

- 按传统 accuracy 协议，zero-shot ChatGPT 只有中等表现，远低于在数据集上训练的 SOTA CRS；加外部推荐模型后 OpenDialKG 差距明显缩小，但 ReDial 仍有差距。
- 失败分析发现，100 个少于 3 轮的失败案例中，51% 的用户偏好被标为模糊；另 100 个失败案例中，ChatGPT 有 36% 在做澄清、11% 在 chit-chat，只有 53% 直接推荐。固定协议把这些合理行为当成错误。
- iEvaLM 模拟器明显优于 DialoGPT：在单轮 naturalness 上 iEvaLM 胜率 36% 对 13%，multi-turn naturalness 55% 对 11%；usefulness 也更高。
- 在 iEvaLM 评估下，ChatGPT + MESE 等通用对话推荐方案在互动场景展现出比固定标签匹配更强的表现。作者报告 ChatGPT 的解释通常能抓住主题、演员或风格关联，并具有较好的 persuasiveness。
- 论文的关键 finding 不是“ChatGPT 绝对更强”，而是 evaluation protocol 会系统性低估能澄清问题、解释推荐并主动收集偏好的 CRS。

## Ablation / Error Analysis

free-form 与 attribute-based 两种交互本身是重要消融：前者允许系统主动澄清，后者控制 action space，便于复现和比较。用户模拟行为也拆成 preference talk、feedback、conversation completion。人工 pairwise 结果显示，模拟器自然度和有用性提升并非只来自更长文本，而是来自能根据 persona 和当前轮次改变行为。

主要错误包括数据对话没有显式偏好、系统无法主动追问、ground-truth item 不能代表唯一正确推荐，以及 LLM scorer 对解释的主观判断。作者报告模拟器与标注者 Cohen's kappa 约 0.73，说明仍不能完全代替真人。

## Limitations

模拟用户的 persona 来自 ground-truth item，可能把数据集偏差带入评测；text-davinci-003 的角色扮演能力也不等于真实用户行为。iEvaLM 依赖 API，成本和 prompt 设计会影响结果；persuasiveness 是代理指标，不等同于长期满意度、点击或实际消费。最多 5 轮的设置也没有覆盖更长的真实推荐旅程。

## 两句话总结

论文证明 ChatGPT 在固定标签匹配协议下的“低分”部分来自评测不允许澄清和互动，而不一定来自推荐能力不足。iEvaLM 用 LLM 用户模拟把 CRS 评测改成多轮、可解释的互动过程，但模拟 persona、LLM judge 和短对话仍需真实用户验证。
