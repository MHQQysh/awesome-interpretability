# 500. Teaching Large Language Models to Self-Debug

- **Authors:** Xinyun Chen, Maxwell Lin, Nathanael Schaerli, Denny Zhou
- **Venue / year:** arXiv 2023 (arXiv:2304.05128)
- **Paper:** [arXiv:2304.05128](https://arxiv.org/abs/2304.05128)
- **Tags:** `code-generation` `self-correction` `rationale` `execution-feedback`

## Introduction
Large language models often generate nearly correct programs but fail on a join condition, a language-translation detail, or a semantic edge case. Existing repair methods usually need a separately trained reviewer, human feedback, or unit-test messages. The paper introduces Self-Debugging: teach a model through few-shot demonstrations to explain its own code, inspect execution results when available, decide whether the prediction is correct, and revise it without an external human critic.

## Method / Framework
The procedure has Generation, Explanation, and Feedback steps. The model first produces code, then explains it line by line in natural language in the style of rubber-duck debugging, then receives a simple correctness signal, unit-test output, explanation feedback, or an execution trace. It iterates until the feedback says the answer is correct or the turn limit is reached. For Spider, where unit tests are unavailable, the feedback is based on SQL explanation and execution against the database; for TransCoder and MBPP, unit-test execution provides richer evidence.

The paper tests four feedback variants: Simple, Unit Test (UT), UT plus Explanation, and UT plus Trace. The same prompting framework is applied to text-to-SQL on Spider, C++-to-Python translation on TransCoder, and text-to-Python generation on MBPP. It uses Codex/code-davinci-002, GPT-3.5, GPT-4, and StarCoder, with greedy or sampled initial programs.

## Baselines / Comparisons
Prior trained rerankers include T5-3B plus N-best reranking, LEVER, Coder-Reviewer, and MBR-Exec. Prompt-only baselines use the original model without debugging, and the paper also compares self-debugging with different sample counts and no-execution settings. The comparison distinguishes code explanation from actual execution: an explanation can expose a wrong WHERE/JOIN clause, but a unit test can reveal a concrete mismatch that the model could not infer from text alone.

## Experiments / Findings
On Spider development, Codex baseline is 81.3 and Self-Debugging with explanation is 84.1; GPT-3.5 rises from 71.1 to 72.2 with simple feedback and GPT-4 from 73.2 to 73.6. On TransCoder, Codex improves from 80.4 to 92.5 with UT plus explanation, GPT-3.5 from 89.1 to 92.7, GPT-4 from 77.3 to 90.4, and StarCoder from 70.0 to 76.6. On MBPP, Codex improves from 61.4 to 70.8 with trace, GPT-3.5 from 67.6 to 74.2 with UT plus explanation, GPT-4 from 72.8 to 80.6 with UT, and StarCoder from 47.2 to 53.2 with trace.

The method is sample-efficient. On Spider, debugging one greedy sample reaches about the baseline accuracy obtained with 16 samples, and debugging from eight samples exceeds the baseline with 32 samples; one debugging turn is usually enough. It improves the hardest Spider problems by about 9%. Error analysis finds fixes for wrong WHERE conditions (25.7%), wrong JOIN clauses (14.3%), and other SQL logic errors; on TransCoder/MBPP, many successful fixes address runtime errors and cross-language implementation differences.

## Ablation / Error Analysis
Richer execution-grounded feedback is consistently better than simple feedback. Without unit-test execution, Codex still gains up to 5% and trace feedback remains consistently better, while GPT-4 gains 3.6% on MBPP and only around 1% on other settings. Self-generated explanations alone sometimes produce overconfident, meaningless changes, especially when the initial code is wrong but no external signal exposes it.

## Limitations
Self-debugging requires repeated model calls and, when used, a runnable execution environment, so cost and security risks increase. Code explanation is not guaranteed faithful and can rationalize an incorrect program; execution feedback is only as good as the tests and database. The study focuses on code tasks and few-shot prompts, and does not show that a model can reliably self-correct arbitrary reasoning or high-stakes code.

## Two-sentence summary
Self-Debugging turns a language model's own code explanation and execution feedback into an iterative repair loop, improving code generation without training a separate reviewer. Execution information is the main source of reliable gains, while explanation-only feedback is useful but can remain overconfident and unfaithful.

## Evidence note
Read the full arXiv text, including the Generation/Explanation/Feedback procedure, Spider/TransCoder/MBPP baselines, feedback-format table, sample-efficiency and execution ablations, error-type breakdown, and limitations.

