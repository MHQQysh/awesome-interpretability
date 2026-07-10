# 367. RoCo: Dialectic Multi-Robot Collaboration with Large Language Models

- **Authors:** Zhao Mandi, Shreeya Jain, Shuran Song
- **Venue / year:** arXiv preprint, 2023
- **Paper:** https://arxiv.org/abs/2307.04738
- **Tags:** `llm-robotics` `multi-agent-dialogue` `planning` `interpretable-collaboration`

## Introduction
Multi-robot collaboration requires task decomposition, capability-aware communication, and collision-free motion planning. Traditional systems are task-specific and often need new engineering for every arrangement, while an LLM offers natural-language coordination that can be inspected by a human.

RoCo treats each robot as an LLM agent that discusses the task, produces a subtask and optional task-space waypoints, receives collision/IK feedback, and then delegates validated goals to a centralized multi-arm planner. The interpretability comes from the visible dialogue and plan revision, not from opening the LLM's internal weights.

## Method / Framework
Each prompt contains task context, round history, agent capability, communication instructions, current observation, and optional failure feedback. Agents end responses with a structured proposal, so text is parsed into actions and goals. A centralized RRT-based motion planner converts validated joint-space goals into collision-free trajectories.

RoCoBench contains six tasks: Pack Grocery, Arrange Cabinet, Sweep Floor, Make Sandwich, Sort Cubes, and Move Rope. The benchmark varies sequential versus concurrent decomposition, shared versus asymmetric observations, workspace overlap, robot embodiments, and capabilities. A text-only multi-agent reasoning set tests self-knowledge, communication, and adaptation.

## Baselines / Comparisons
The main baseline is an oracle “Central Plan” GPT-4 agent with full environment observation and all robot capabilities. Ablations remove dialog/history or environment feedback. For waypoint planning, the comparisons are linear interpolation and a hard-coded top-down pick/place path.

The paper also compares GPT-4-0314, GPT-4-0613, GPT-3.5-turbo, and Claude-v1 on the reasoning dataset. These comparisons separate the benefit of the RoCo protocol from the general language model's ability to answer capability, memory, inquiry, response, and adaptation questions.

## Experiments / Findings
On RoCoBench, full Dialog succeeds at 0.44/0.75/0.95/0.80/0.93/0.65 on the six tasks, respectively. The oracle Central Plan is stronger on several tasks, but Dialog can match or exceed it on Sort Cubes because the robots discover a cooperative strategy that satisfies constraints the centralized prompt mishandles. Removing feedback hurts Pack Grocery and Make Sandwich, while removing history can cause catastrophic failure on Sweep Floor.

LLM-proposed waypoints show no clear advantage for picking but significantly accelerate planning for placing, where arm-object collisions are more likely. In the text reasoning set, GPT-4-0613 scores 0.68 capability, 0.91 memory, 0.57 inquiry, 0.86 response, and 0.71 adaptation; GPT-3.5 is notably weaker on communication and inquiry.

The real-robot study demonstrates human-in-the-loop collaboration and adaptation to object initialization and task-order variations. The dialogue provides a readable trace of who proposed which subtask and why a plan was revised.

## Ablation / Error Analysis
History and feedback are direct prompt ablations. Full dialogue is not uniformly best on every individual task, but it is best overall because it preserves both memory of previous actions and reasons for replanning. The waypoint ablation isolates task-space reasoning from the downstream motion planner.

Failures come from imperfect perception, open-loop execution, parser/plan mismatches, and LLM response delay. In simulation RoCo assumes oracle object state and collision checking; in the real world, object detector errors can produce invalid plans that the language layer cannot repair at execution level.

## Limitations
The method relies on accurate scene descriptions, expensive repeated LLM queries, and static or slowly changing tasks. Open-loop trajectories do not handle execution-level feedback, and natural-language parsing can fail. The benchmark is broad but still simulation-heavy, so success rates do not establish reliable general-purpose robot control.

## 两句话总结
RoCo 让每个机器人通过 LLM 对话分配子任务、生成 waypoint，并用碰撞/IK 反馈反复修正，再交给集中式 RRT 做低层规划，因此协作过程天然可读。六任务 RoCoBench 显示对话和反馈能提升泛化，但感知、开放环执行、查询延迟和错误解析仍是实际部署的主要瓶颈。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；RoCoBench 表 2 的六任务成功率和多模型 reasoning 表 4 按原文记录。
