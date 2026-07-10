# 339. Transformers Use Causal World Models in Maze-Solving Tasks

- **Authors:** Alex F. Spies, William Edwards, Michael Ivanitskiy, Adrians Skapars, Tilman Räuker, Katsumi Inoue, Alessandra Russo, Murray Shanahan
- **Venue / year:** ICLR 2025
- **Paper:** https://arxiv.org/abs/2412.11867
- **Tags:** `world-models` `maze` `sparse-autoencoder` `causal-intervention` `positional-encoding`

## Introduction
The paper studies whether a Transformer trained on a controlled planning task forms an internal world model: a representation that preserves the maze's structure and is causally used to select a path. Synthetic mazes are chosen because the environment is human-readable, connectivity is controllable, and the shortest path is unique. This lets the authors separate “the model can solve the task” from “the model represents the task's causal structure.”

Earlier maze work used probes or attention patterns but did not isolate causal features cleanly. The authors combine attention analysis, SAEs, and interventions, and compare learned positional embeddings with rotary positional embeddings. This comparison becomes important because a model can appear to understand a maze while actually relying on sequence positions that fail under length extrapolation.

## Method / Framework
Mazes are generated on grids up to 7x7 using constrained randomized depth-first search. The adjacency list encodes connections with coordinate tokens separated by semicolons; origin, target, and the solution path follow. Models are trained on 5x5 fully connected and 6x6 sparsely connected mazes embedded in a 7x7 vocabulary, so 7x7 and longer connection lists test generalization.

The authors inspect early attention heads and find that layer-0 heads attend from semicolon tokens back one or three positions to the coordinates of the preceding connection. They then train SAEs on the residual stream after the first block. Decision trees identify sparse features that encode specific maze connections. The attention-derived and SAE-derived features are compared, and both are causally tested by patching or toggling features at semicolon positions.

Two representative 6-layer, 512-dimensional models are studied: Stan with learned positional embeddings and 8 heads, and Terry with rotary embeddings and 4 heads. Their maze-solving accuracies are 96.6% and 94.3%. An intervention adds a connection by activating a corresponding SAE feature or removes one by zeroing it, then checks whether the generated path changes as the ground-truth maze changes.

## Baselines / Comparisons
The paper does not use a conventional task-model leaderboard. Its comparisons are: attention-head analysis versus SAE feature analysis; feature interventions versus unperturbed SAE reconstructions; learned-position Stan versus rotary-position Terry; and calculated-value interventions versus fixed-value interventions. Linear-probe-style assumptions are also contrasted with SAE discovery, because SAEs can isolate features without specifying the world-model ontology in advance.

## Experiments / Findings
The first finding is that both models build connectivity information at semicolon tokens. In Terry, all four layer-0 heads show the characteristic pattern; in Stan, three of eight heads do. The SAE features line up with these attention circuits: individual features encode particular grid connections, and the residual SAE reconstruction has little effect on the model's normal path, suggesting that the SAE preserves relevant information.

The second finding is causal. Across 200 examples per connection feature, activating features is consistently more effective than removing them. In Terry, turning on a feature can make the model behave as if a missing connection exists, while the path changes in the corresponding direction. This asymmetry suggests that the Transformer relies more strongly on the presence of connectivity cues than on explicit absence.

The third finding concerns positional encodings. Stan fails when given input sequences with more connections than seen in training, so removal interventions fail partly because the model cannot process the longer sequence. Yet activating the latent SAE feature can still produce coherent behavior in the latent space; the representation has some generalization after it is decoupled from the learned position embedding. Terry generalizes better because rotary positions are less tied to the training sequence length.

The paper also finds that Stan uses a more compositional code. Activating connection-specific features at unrelated semicolons succeeds in about 35% of cases, whereas removing features fails on the longer inputs. Fixed-value interventions are weaker than interventions using values calculated from the feature statistics, showing sensitivity to the magnitude and context of the SAE feature.

## Ablation / Error Analysis
The positional-encoding comparison is the main error analysis. A low intervention success rate does not automatically mean the feature is non-causal: learned positional embeddings can prevent the model from handling the altered sequence before the feature intervention is even used. The authors therefore compare unperturbed model output, SAE reconstruction output, and modified SAE output.

The SAE sweep varies expansion factor, sparsity, learning rate, and ghost threshold. Final SAEs have low reconstruction error and small feature density; replacing the residual stream with the SAE reconstruction changes the generated sequence little. Fixed-value additions are less effective than setting a feature to a value calculated from observed activations, indicating that magnitude matters.

## Limitations
The environment is synthetic and acyclic, with unique paths, so the result does not prove that frontier LLMs learn real-world causal world models. There are only two selected Transformer variants, and SAE feature quality depends on dictionary hyperparameters and layer choice. The intervention asymmetry may be task-specific. Finally, a causal feature in a maze solver demonstrates a useful representation, but it does not by itself show that the model has a human-like world model or robust planning beyond the generated distribution.

## 两句话总结
这篇论文在可控迷宫任务中用 attention、SAE 和 causal intervention 证明 Transformer 会形成编码连接关系的内部 world model，并且这些 feature 真的会改变路径行为。它同时揭示 learned positional embedding 会造成长度泛化失败，而 latent-space feature activation 仍能部分恢复行为，因此“不能在输入序列上泛化”不等于“内部表示没有泛化”。

## Evidence note
This note was written from the ICLR 2025 PDF (arXiv:2412.11867v2), including the maze construction, Stan/Terry setup, attention/SAE discovery, intervention Figure 9, fixed-value ablation, and conclusion.
