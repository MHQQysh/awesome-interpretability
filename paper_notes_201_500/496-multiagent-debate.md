# 496. Improving Factuality and Reasoning in Language Models through Multiagent Debate

- **Authors:** Yilun Du, Shuang Li, Antonio Torralba, Joshua B. Tenenbaum, Igor Mordatch
- **Venue / year:** arXiv 2023 (arXiv:2305.14325)
- **Paper:** [arXiv:2305.14325](https://arxiv.org/abs/2305.14325)
- **Tags:** `reasoning` `factuality` `multi-agent` `self-correction`

## Introduction
Single language-model samples can be wrong, and a single model's reflection prompt can simply restate the same mistake. The paper asks whether several instances can act like debaters: independently produce solutions, inspect one another's reasoning, and revise toward a shared answer. The intended explanation is not that one agent is always right, but that disagreement exposes uncertainty and creates a channel for error correction.

## Method / Framework
Multiagent debate initializes several language-model agents with the same question and lets each produce an answer and reasoning. At every round, each agent receives the other agents' responses and an instruction to critically update its own answer; the process repeats until a final answer is selected. Prompts can induce short or long debates, and agents can be distinct personas or distinct model families. The method is black-box and does not require model retraining.

The experiments combine ordinary generation with zero-shot Chain-of-Thought, and also test response summarization when many agents make the context too long. Debate is evaluated with two or three agents and multiple rounds, while majority voting and single-agent reflection serve as explicit alternatives. The paper separately studies mathematical reasoning, chess move validity, MMLU factual questions, and a new biography-factuality task built from 524 computer scientists.

## Baselines / Comparisons
The reasoning baselines are a single agent, single-agent majority/consistency-style generation, and single-agent reflection. The factuality setting compares single-agent generation with single-agent reflection and omits simple majority because free-form biography answers are not directly comparable. The authors also compare debate with and without CoT, change the number of agents and rounds, vary debate prompt length, use different personas, and run a small ChatGPT/Bard cross-model debate.

## Experiments / Findings
With three agents and two debate rounds, arithmetic accuracy is 81.8 +/- 2.3, GSM8K accuracy is 85.0 +/- 3.5, and chess pawn score is 122.9 +/- 7.6. In the factuality table, single-agent scores are 66.0 +/- 2.2 on biographies, 63.9 +/- 4.8 on MMLU, and 29.3 +/- 2.6 on chess move validity; reflection gives 68.3/57.7/38.8, while debate reaches 73.8/71.1/45.2. A small ChatGPT/Bard experiment solves 17 of 20 GSM8K questions after debate versus 14 for ChatGPT and 11 for Bard alone.

Qualitative traces show that all agents can initially be wrong and still converge to the correct answer after exchanging derivations. Agents are more likely to change their minds when they disagree, while high-confidence repeated answers are harder to persuade. Increasing agent count and rounds generally helps; extra rounds beyond about four saturate. Different personas improve MMLU from 71.1 to 74.2, and summarizing other agents' outputs helps control context length.

## Ablation / Error Analysis
The number-of-agents and number-of-rounds sweeps establish that debate is not just majority voting. Short versus long prompts trade faster consensus against more reasoning, with longer debates usually more accurate. Removing execution-like external verification leaves debate dependent on language-model judgment, and very long debate contexts cause agents to focus on recent responses rather than the whole history.

## Limitations
Debate multiplies inference cost and latency by the number of agents and rounds. It can converge to a confident but false consensus, and the authors explicitly observe that models can be agreeable and that longer inputs exceed effective context processing. The method is evaluated on modest task sets and does not establish that debate is robust to adversarial collusion, correlated model errors, or a knowledgeable but outnumbered minority.

## Two-sentence summary
Multiagent debate improves reasoning and factuality by exposing disagreement and letting multiple model instances critique and revise their answers. The gain is consistent across arithmetic, GSM8K, biographies, MMLU, and chess, but it costs several inference calls and can still settle on an incorrect consensus.

## Evidence note
Read the full arXiv text, including debate prompts, arithmetic/GSM8K/chess tables, 524-biography benchmark, MMLU results, agent/round/prompt sweeps, summarization analysis, and limitations.

