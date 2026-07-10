# 335. Can LLMs Produce Faithful Explanations For Fact-checking? Towards Faithful Explainable Fact-Checking via Multi-Agent Debate

- **Authors:** Kyungha Kim, Sangyun Lee, Kung-Hsiang Huang, Hou Pong Chan, Manling Li, Heng Ji
- **Venue / year:** arXiv 2024 (arXiv:2402.07401)
- **Paper:** https://arxiv.org/abs/2402.07401
- **Tags:** `faithfulness` `fact-checking` `multi-agent` `debate` `hallucination`

## Introduction
Fact-checking systems need more than a true/false label. A user must understand which evidence supports the verdict, especially when a claim requires reasoning over several documents. LLMs are good at producing fluent explanations, but fluency can hide unsupported entities, events, or generalizations. The paper's first question is therefore empirical: how faithful are zero-shot LLM explanations when compared with the supplied evidence? Its second question is how to improve faithfulness without relying on a single self-critique pass.

The authors define faithfulness as factual consistency between the generated explanation and the evidence. They build an error typology covering intrinsic and extrinsic entity-related, event-related, and noun-phrase-related errors, plus reasonability and connected-evidence errors. This typology is used both for human annotation and as structured guidance for automatic evaluation.

## Method / Framework
MADR, Multi-Agent Debate Refinement, assigns four roles to LLM agents: two debaters, a judge, and a refiner. A zero-shot generator first creates an explanation from the claim, label, and evidence. Debater 1 analyzes the explanation using the predefined error typology; Debater 2 independently searches for faithfulness problems without being limited to that typology. Their feedback is compared by the judge.

If the feedback does not agree, each debater reviews the other's feedback and revises its own list. The process repeats for a fixed number of rounds or until the judge finds agreement. The final feedback is concatenated and given to the refiner, which is instructed to change only the sentences implicated by the feedback. The key difference from ordinary debate is that debate generates validated feedback for a later refinement step; it does not directly let agents overwrite the explanation at every turn.

## Baselines / Comparisons
The baselines are Zero-shot, Chain-of-Thought (CoT), and Self-Refine. Zero-shot directly writes an explanation. CoT asks the LLM to expose a reasoning process before the final explanation. Self-Refine generates one explanation and repeatedly asks the same agent to improve it. All experiments use GPT-3.5-Turbo for a controlled comparison.

The dataset is PolitiHop, whose 445 test instances contain a claim and multiple evidence pieces. Automatic evaluation uses GPT-4-Turbo G-Eval under four protocols: sentence-level versus document-level, and with versus without the error typology. Human evaluation uses four qualified Mechanical Turk annotators, with Cohen's kappa 0.69 against an author annotation.

## Experiments / Findings
On the four automatic protocols, MADR scores 4.82, 4.99, 4.88, and 4.97. It is best on two of the four columns, but the larger lesson is that these near-ceiling automatic scores are not reliable evidence of faithfulness. In a 20-sample human evaluation, only 20% of Zero-shot explanations are judged faithful, CoT falls to 5%, Self-Refine is 20%, and MADR reaches 30%. The corresponding error counts are 25, 42, 32, and 17.

The paper therefore exposes a judge problem. GPT-4-Turbo's automatic evaluations are close to 5 even when human annotators find hallucinated details. Correlation with human judgments is highest for sentence-level evaluation with the typology (Kendall's tau 0.150), followed by document-level with typology (0.128), rather than evaluation without the error categories. Fine-grained, evidence-aware judging is better aligned than a single holistic score.

The qualitative MADR case shows why two perspectives help. One debater catches an incorrect name in a sentence; the other independently notices that the sentence attributes an event to the wrong person. Their agreement produces actionable feedback to replace the entity rather than merely asking for a more convincing explanation. Self-Refine can miss exactly this type of anchored error because its own prior text becomes the feedback context.

## Ablation / Error Analysis
The principal comparison is the self-refinement ablation. Adding CoT does not solve faithfulness and in the human study actually has the highest error count. Self-Refine improves neither the percentage of faithful explanations nor the number of errors relative to Zero-shot. MADR's gain comes from role separation, independent error search, typology-aware review, and a judge that forces feedback agreement.

The automatic-evaluation ablation compares granularity and typology. Adding the error typology improves correlation with human labels, but it does not by itself eliminate the gap between GPT scoring and human judgment. The authors also set a maximum debate iteration to prevent endless agent interaction, so the method trades extra calls and cost for more controlled refinement.

## Limitations
The study is limited to the English PolitiHop test set and GPT-3.5 generation, so it does not establish that the same role prompts transfer to other fact-checking domains, languages, or model families. Prompt sensitivity is not systematically explored. The automatic judge still uses an LLM and can hallucinate or overlook evidence, while the human study is small. Finally, multi-agent debate increases inference cost and latency, which matters for real-time misinformation response.

## 两句话总结
MADR 用两个独立 debater、一个 judge 和一个 refiner，把“发现证据不一致”与“修改解释”分开，并通过多轮反馈一致性减少幻觉细节。论文最有价值的发现是 zero-shot/CoT 的流畅解释并不等于 faithful，且带错误类型的细粒度评估比单一 LLM judge 更接近人工判断。

## Evidence note
This note was written from the paper PDF (arXiv:2402.07401v1), including the error typology, Algorithm 1, PolitiHop setup, Tables 2-4, human-evaluation protocol, and limitations.
