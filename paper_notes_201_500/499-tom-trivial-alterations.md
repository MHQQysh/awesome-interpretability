# 499. Large Language Models Fail on Trivial Alterations to Theory-of-Mind Tasks

- **Authors:** Tomer D. Ullman
- **Venue / year:** arXiv 2023 (arXiv:2302.08399)
- **Paper:** [arXiv:2302.08399](https://arxiv.org/abs/2302.08399)
- **Tags:** `theory-of-mind` `behavioral-evaluation` `robustness` `reasoning`

## Introduction
Theory of Mind (ToM) requires attributing beliefs, goals, perceptual access, and false beliefs to other agents. A prior study reported that LLMs could solve classic false-belief-like prompts at a level comparable to children. This paper tests whether that success is robust: if a tiny story change preserves the ToM principle but changes a superficial cue, a system with functional belief reasoning should preserve the answer.

## Method / Framework
The paper uses GPT-3.5, focusing on the then-current model version that achieved the strongest prior result. It reproduces belief-attribution prompts and then applies directed perturbations rather than random paraphrases. Variants alter perceptual access, transparency, labels, the protagonist's ability to read a label, the location of an object, or which person is queried, while preserving the relevant mental-state logic.

The analysis compares the model's token probabilities and completions for content prompts and belief prompts. It also studies classic Sally-Anne-style stories with a person who moves an object and asks where another person will search. The aim is not to train a new model, but to use adversarially minimal interventions to distinguish memorized answer patterns from robust relational reasoning.

## Baselines / Comparisons
The baseline is the original task formulation and the success claim from Kosinski's evaluation. Each minimally changed vignette is compared with its unperturbed counterpart; the paper does not compare many model families or report a conventional aggregate benchmark leaderboard. The relevant control is semantic invariance: a correct ToM solver should answer the same belief question consistently when irrelevant surface details change.

## Experiments / Findings
In the reproduced popcorn/chocolate bag example, GPT-3.5 assigns the expected content answer to the direct fact prompt and flips to chocolate for the belief prompt. But trivial changes such as making the bag transparent or putting a label on it cause systematic failures: the model attributes the wrong belief even when the protagonist's information state is unchanged. One variant makes the label unreadable to the protagonist; the label should then be irrelevant, yet GPT-3.5 still follows it.

In object-location stories, the model can pass the original false-belief prompt while failing when the object's location, perceptual access, or queried person's role is changed. The same answer can be copied across agents who have different observations, suggesting sensitivity to lexical templates or the common “look where it is not” pattern rather than a stable belief-state representation. The paper's qualitative conclusion is that GPT-3.5 produces reasonable responses on familiar ToM vignettes but is not robustly solving the underlying task.

## Ablation / Error Analysis
The perturbations are the ablation: transparency, labels, inability to read, object relocation, and query-target changes. They are deliberately small and ToM-preserving, so the resulting flips are stronger evidence than a low average score alone. The author cautions that a failure could reflect ToM, scene understanding, relational reasoning, or language interpretation, but all explanations undermine the claim that passing the original template demonstrates spontaneous human-like ToM.

## Limitations
The study concentrates on GPT-3.5 and a small set of hand-designed stories, so it cannot estimate the behavior of later models or all forms of social reasoning. Prompt wording and tokenization can affect probabilities, and a model may improve through targeted exposure without acquiring the human cognitive mechanism. The paper is a robustness critique, not a proof that no LLM can ever represent beliefs.

## Two-sentence summary
GPT-3.5 can answer familiar false-belief prompts but fails under tiny, principle-preserving changes that should not matter to a genuine ToM solver. The paper argues that outlying failures and invariance tests deserve more weight than average success on benchmark templates, because memorized surface patterns can imitate belief reasoning.

## Evidence note
Read the full arXiv text, including the reproduced Kosinski-style prompts, directed perturbation cases, object-location variants, discussion of formal versus functional ToM, and evaluation cautions.

