# 149. Theory of Mind for Multi-Agent Collaboration via Large Language Models

- **Authors:** Huao Li, Yu Quan Chong, Simon Stepputtis, Joseph Campbell, Dana Hughes, Michael Lewis, Katia Sycara
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.13
- **Tags:** Theory of Mind, Multi-Agent, Collaboration, Belief State

## Introduction

静态 false-belief 测试中 GPT-4 可能表现接近儿童，但真实合作需要动态更新多个 agent 的 belief、意图、观察和通信。论文设计一个 embodied text game，在未知环境中要求多 LLM agent 探索、规划和合作，并比较 LLM agent 与 MARL、planning baseline。

## Method / Framework

每个 agent 通过文字观察环境和其它 agent 的行动，维护任务状态并回答高阶 ToM 问题，例如“另一个 agent 认为目标在哪儿”。作者比较普通 ChatGPT / GPT-4 prompt 与显式 belief state representation：把世界状态、历史观察、通信和对其它 agent 的信念结构化后放入下一轮 prompt。

评价包括团队任务得分、非法 action 数、长时规划表现，以及在任务进程中针对一阶 / 二阶 belief 的 ToM inference accuracy。

## Baselines / Comparisons

比较 GPT-4、ChatGPT、GPT-4 + belief state、MARL agent、planning-based optimal / heuristic agent。ToM baseline 主要是只给自然语言历史的模型，belief-state 版本用于判断显式结构是否能修复长上下文和状态记忆问题。

## Experiments / Findings

- LLM agent 展现出 emergent collaborative behaviors，例如通过通信分工、分享观察和推断队友目标，但普通 ChatGPT team 在任务完成上明显失败。
- GPT-4 + belief state 的 team score 达到 43±4.7，普通 GPT-4 / ChatGPT 低于它；所有 LLM agent 仍不如具有完整状态和规划能力的 optimal baseline。
- ToM inference 中，GPT-4 + belief 的准确率约 79.0%，普通 GPT-4 约 41.9%，ChatGPT 约 11.6%（表 2），说明显式 belief 结构比单纯增加模型能力更直接。
- 主要系统性失败有两类：长 horizon 中无法保持早期关键事实，以及 hallucinate 不可执行动作或错误房间状态；这些错误会快速传播到整个团队。
- belief state representation 减少 invalid action，并提高任务完成和 ToM 判断，说明可解释的中间状态不仅帮助分析，也能帮助控制行为。

## Ablation / Error Analysis

普通 prompt、belief state prompt、不同通信历史和任务长度是主要消融。LLM 有时会把自己的 belief 当成真实世界状态，或者把未观察到的事实当成已知；在二阶 ToM 中，模型能复述部分历史，却漏掉“谁知道什么”的嵌套关系。自然语言 ToM 指标还依赖人工标注者对复杂状态的解释。

## Limitations

实验是一个文本化任务，不等同于真实多智能体环境；团队只含 3 个 agent，任务规模和 horizon 有限。belief state 由 prompt 中显式提供，不能证明模型在内部自主维护了同样结构。模型版本、提示格式和人工设计的环境规则会影响结果，尚未评估人机混合团队和更大 agent 数量。

## 两句话总结

论文把 Theory of Mind 放进动态多 agent 合作游戏，发现普通 LLM 能产生部分协作行为，却在长时状态管理和 hallucinated action 上失效。把世界和他人 belief 显式写入 prompt 后，任务表现和 ToM accuracy 都提升，说明结构化中间状态是比“只让模型更聪明”更可操作的解释与控制手段。
