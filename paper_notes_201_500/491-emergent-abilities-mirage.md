# 491. Are Emergent Abilities of Large Language Models a Mirage?

- **Authors:** Rylan Schaeffer, Brando Miranda, Sanmi Koyejo
- **Venue / year:** arXiv 2023 (arXiv:2304.15004)
- **Paper:** [arXiv:2304.15004](https://arxiv.org/abs/2304.15004)
- **Tags:** `emergent-abilities` `evaluation` `scaling` `interpretability`

## Introduction
The paper challenges a popular interpretation of scaling curves: that a capability is absent in small models and then appears suddenly and unpredictably in a larger model. The authors separate the model's actual token-level behavior from the metric used to summarize a complete answer. Their central hypothesis is that a smooth decrease in per-token error can look like a phase transition when evaluated by exact match, multiple-choice grade, or another nonlinear/discontinuous metric.

## Method / Framework
The paper first gives a mathematical toy model. If the probability of getting one token right changes smoothly with scale, the probability of getting every token in a long target right can fall approximately geometrically with target length. Thus exact sequence accuracy amplifies small token-level changes. A discontinuous metric such as Multiple Choice Grade can create a step even when the underlying probability mass changes continuously.

The authors test three predictions. (1) Holding model outputs fixed and replacing Accuracy with Token Edit Distance should remove the apparent emergence. (2) Increasing the number of test examples should reveal above-chance, smoothly improving accuracy that was hidden by low statistical resolution. (3) Changing target length should have predictable effects: approximately geometric for exact accuracy and roughly quasilinear for edit distance.

## Baselines / Comparisons
The main comparison is not a competing model but a controlled metric comparison. The study uses the InstructGPT/GPT-3 family on two-shot two-digit multiplication and two-shot four-digit addition, and compares Accuracy with Token Edit Distance. It then performs a BIG-Bench meta-analysis across task, metric, and model-family triplets, contrasting discontinuous Multiple Choice Grade and nonlinear Exact String Match with continuous metrics such as Brier Score. Finally, it repeats the idea in shallow nonlinear autoencoders, convolutional networks, and autoregressive/self-attentional vision models.

## Experiments / Findings
On the arithmetic tasks, the same GPT-family outputs look sharply emergent when scored by exact Accuracy for four- or five-digit targets, but improve smoothly and predictably under Token Edit Distance. Increasing the target length produces the expected geometric degradation under Accuracy and quasilinear degradation under edit distance. With a larger evaluation set, even the small models show nonzero above-chance exact accuracy, and the curve follows the predicted smooth trend rather than a true zero-to-nonzero transition.

The BIG-Bench analysis finds that at most five of 39 preferred metrics show possible emergence. In hand-annotated claims, only four metrics are involved, and Multiple Choice Grade plus Exact String Match account for more than 92% of the reported emergent abilities. For LaMDA tasks that appear emergent under Multiple Choice Grade, replacing it with continuous Brier Score makes the emergence disappear. The vision experiments show that the same researcher-controlled effect can be manufactured in several architectures, including a new discontinuous reconstruction metric.

## Ablation / Error Analysis
The decisive ablations are metric substitutions and resolution increases. They keep model outputs fixed while changing only the measurement, which rules out a change in the model as the immediate cause of the visual transition. The authors also vary target-string length and show that the changes follow the metric's known aggregation behavior. A remaining risk is multiple comparisons: BIG-Bench combines hundreds of tasks, dozens of metrics, and many model families, so some apparently sharp curves are expected by selection alone.

## Limitations
The paper does not prove that every capability claim is an artifact; a genuine discontinuous change in a model could exist. Its strongest evidence concerns claims measured with exact-match or threshold-like metrics and does not replace task-specific causal analysis of the learned computation. Continuous metrics can also be less aligned with the actual user goal, so metric redesign must preserve task validity rather than simply smooth the curve.

## Two-sentence summary
Many alleged emergent abilities can be explained by nonlinear or discontinuous metrics amplifying smooth token-level improvements, together with insufficient evaluation resolution. The paper therefore recommends reporting calibrated continuous metrics, adequate sample sizes, and metric-sensitive scaling analyses before interpreting a sharp curve as a new internal capability.

## Evidence note
Read the full arXiv text, including the mathematical model, GPT-3 arithmetic tests, BIG-Bench analysis, vision induction experiments, and discussion of metric-induced emergence.

