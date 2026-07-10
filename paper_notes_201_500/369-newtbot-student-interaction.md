# 369. Student Interaction with NewtBot: An LLM-as-tutor Chatbot for Secondary Physics Education

- **Authors:** Anna Lieb, Toshali Goel
- **Venue / year:** CHI 2024
- **Paper:** https://doi.org/10.1145/3613905.3647957
- **Tags:** `education` `human-study` `llm-tutor` `user-experience`

## Introduction
LLM tutors may personalize help, but education requires accuracy, productive engagement, and an interaction that students trust. The paper introduces NewtBot, a physics tutoring chatbot for secondary-school tasks, and studies how different prompt configurations affect students' experience and willingness to use the system.

The interpretability angle is user-facing rather than mechanistic: the tutor's behavior is configured by explicit backend prompts, and the study examines whether students perceive a setting-specific tutor/feedback behavior as more useful than a generic chatbot.

## Method / Framework
NewtBot is a web interface whose GPT-3.5 backend can be prompted into three configurations: a general-purpose baseline, a setting-specific tutor, and a problem-specific feedback model. The models are used while German secondary-school students work on physics problems. The interface and backend behavior are compared in a user study rather than trained as a new foundation model.

## Baselines / Comparisons
The three backend configurations are the main baselines. The general-purpose GPT-3.5 model supplies a generic conversational reference; the tutor prompt encodes a physics-teaching role; the feedback prompt focuses on the particular problem and response. The paper compares user experience and perceived usefulness, not only answer accuracy.

## Experiments / Findings
The accessible study record reports 50 German secondary-school participants. Students had overall positive experiences, and the setting-specific tutor configuration received the highest user-experience ratings. Although 72% expressed apprehension about using chatbots for school, 70% said they would use NewtBot to help with physics work.

These findings suggest that explicit role and task framing can make the same underlying LLM feel more like a tutor and less like an unconstrained answer generator. The result is about acceptance and interaction design; it does not establish that the tutor configuration is more factually correct or pedagogically effective over time.

## Ablation / Error Analysis
The three prompt configurations function as a behavior ablation, but the accessible record does not expose the full task-level accuracy tables, dialogue coding scheme, or error taxonomy. The central risk is that positive user experience can coexist with incorrect physics explanations, over-reliance, or reduced productive struggle.

## Limitations
The publisher full text was not available in the local corpus, so this note is limited to the DOI metadata and the accessible abstract-level study record. The exact survey instruments, statistical tests, physics tasks, prompt text, and factual-error analysis still require the CHI paper PDF. Results are also from one educational context and GPT-3.5 configuration.

## 两句话总结
NewtBot 用同一个 GPT-3.5 后端的 generic、setting-specific tutor 和 problem-specific feedback 三种 prompt 配置，为中学物理学习提供个性化辅导。50 名德国中学生总体体验积极且最偏好 tutor 配置，但现有摘要证据不能证明其物理正确性、长期学习收益或解释忠实性。

## Evidence note
证据边界：当前未获得 CHI 2024 出版商全文；人数、三种配置、72%/70% 态度数据来自公开摘要/索引记录，未补写不可核验的 baseline 数字和统计检验。
