# 492. Large Language Models as Zero-Shot Conversational Recommenders

- **Authors:** Zhankui He, Zhouhang Xie, Rahul Jha, Harald Steck, Dawen Liang, Yesu Feng, Bodhisattwa Prasad Majumder, Nathan Kallus, Julian McAuley
- **Venue / year:** CIKM 2023
- **Paper:** [arXiv:2308.10053](https://arxiv.org/abs/2308.10053) / [DOI](https://doi.org/10.1145/3583780.3614949)
- **Tags:** `recommendation` `zero-shot` `probing` `shortcut-learning`

## Introduction
Conversational recommendation differs from ordinary recommendation because the system must use multi-turn natural language, infer preferences, and produce both recommendations and conversational justification. Existing CRS benchmarks are mostly crowdsourced and often label an item repeated from earlier turns as the ground-truth recommendation. The paper argues that this creates a repeated-item shortcut: copying a mentioned item can score well even when the system is only discussing it.

## Method / Framework
The proposed system is deliberately simple. A language model receives a task description T, an output-format requirement F, and the conversation context S; a post-processor Phi converts generated movie titles into a ranked item list. Models are run at temperature zero and include GPT-3.5-turbo, GPT-4, BAIZE, and Vicuna. The study constructs Reddit-Movie by processing Reddit movie conversations from five subreddits, linking movie mentions to entities, and evaluates the most recent 9k conversations from the 2022 subset while training conventional baselines on the remaining data.

The diagnostic framework separates repeated items from new items and perturbs the conversation in four ways: Original, ItemOnly, ItemRemoved, and ItemRandom. ItemOnly tests collaborative/item knowledge, while ItemRemoved preserves verbal preferences, genres, actors, and context and tests content/context knowledge. The paper also measures dataset information using entropy of 1-, 2-, and 3-gram distributions after removing item names.

## Baselines / Comparisons
The retrained CRS baselines are ReDIAL, KBRD, KGSF, and UniCRS; the analysis additionally uses BERT-small text encoding, ItemCF/FISM-style collaborative filtering, and a trivial copy-seen-items baseline. The LLM comparison includes closed models GPT-3.5/GPT-4 and open models BAIZE/Vicuna. Evaluation reports Recall@1 and Recall@5 under standard and new-item-only protocols, with both all-generated-title and in-dataset-title post-processors.

## Experiments / Findings
Reddit-Movie contains 634,392 conversations, 1,669,720 turns, 36,247 users, and 51,203 movies; its 2022 base subset contains 85,052 conversations and 24,326 movies. Under the ordinary protocol, a copy-seen-items baseline can beat most prior CRS systems, and removing repeated items causes the tested models' Hit@1 to fall by more than 60% on average. After removing the shortcut and retraining baselines, zero-shot LLMs are best on INSPIRED, ReDIAL, and Reddit; GPT-4 is generally better than GPT-3.5 and both beat BAIZE/Vicuna.

The reported Recall@1 values for generated-title post-processing are GPT-3.5/GPT-4 = .047/.062 on INSPIRED, .041/.043 on ReDIAL, and .022/.022 on Reddit. Restricting output to in-dataset titles gives GPT-3.5/GPT-4 = .052/.066, .043/.046, and .023/.023 respectively. About 95.51% of GPT-3.5 and 94.86% of GPT-4's top-20 recommendations string-match IMDB; the lower rates for BAIZE/Vicuna are 81.56%/86.98%.

The probing results show that GPT models lose more than 60% of Recall@5 when only item mentions remain, but lose less than 10% on average when item mentions are removed or randomized. On ItemRemoved, GPT-4 reaches Recall@1/5 of .062/.128 on INSPIRED, .032/.102 on ReDIAL, and .019/.075 on Reddit, above the retrained CRS and text-encoder baselines in that setting. On ItemOnly, CRS/ItemCF models are generally about 30% better than LLMs on INSPIRED and ReDIAL, while Reddit is an exception because it has many rarely interacted items; 12,982 items occur no more than three times as response items.

## Ablation / Error Analysis
The key ablation is the repeated-item removal, which changes the conclusion of the benchmark without changing the model. The ItemOnly versus ItemRemoved intervention isolates collaborative from linguistic knowledge; the ItemRandom control checks whether replacing items merely damages grammar. Further analysis finds Reddit has more content/context information than the other CRS datasets and is closer to conversational search and QA. The study also identifies popularity bias and geography sensitivity: popular English-speaking-region movies are over-recommended and GPT-4 performs better for movies from English-speaking regions.

## Limitations
Movie titles and IMDB string matching make hallucination estimates only lower bounds, and the study cannot inspect closed-model weights. The definition of a new versus repeated recommendation depends on entity linking and conversation annotations. Reddit is a movie-heavy English platform, so its content-rich setting may overstate zero-shot LLM performance for other cultures, domains, or recommendation interfaces.

## Two-sentence summary
Zero-shot GPT models can outperform fine-tuned conversational recommenders once the benchmark's repeated-item shortcut is removed, largely because they understand rich textual preferences. Their advantage is content/context knowledge rather than collaborative interaction knowledge, and it is accompanied by popularity, regional, and dataset-distribution biases.

## Evidence note
Read the full arXiv/CIKM text, including dataset statistics, prompting equation, new-item figures, title-matching table, ItemOnly/ItemRemoved probes, entropy analysis, and limitations section.

