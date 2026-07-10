# 337. Sparse Feature Circuits: Discovering and Editing Interpretable Causal Graphs in Language Models

- **Authors:** Samuel D. Marks, Can Rager, Eric J. Michaud, Yonatan Belinkov, David Bau, Aaron Mueller
- **Venue / year:** ICLR 2025
- **Paper:** https://arxiv.org/abs/2403.19647
- **Tags:** `sparse-autoencoder` `feature-circuits` `causal-graphs` `SHIFT` `automated-interpretability`

## Introduction
Mechanistic circuit work often identifies attention heads, MLPs, or neurons, but these units are frequently polysemantic. A circuit made of such coarse units can be causally useful while remaining difficult for a human to interpret or edit. The paper asks whether circuits can instead be discovered over sparse, human-interpretable SAE features, including the SAE reconstruction-error nodes needed to preserve information lost by the dictionary.

There is a second scalability problem. Existing circuit analyses usually start with researcher-curated tasks and manually chosen contrastive examples. The authors want a pipeline that can discover circuits for unexpected behaviors and at much larger scale, while still supporting causal validation rather than only feature visualization.

## Method / Framework
The method trains sparse autoencoders on model activations. For each model component, an activation is decomposed into sparse feature activations plus a reconstruction error. The circuit discovery procedure then: caches activations and a task metric, filters nodes by their causal effect, and filters edges by the effect of transmitting information between selected nodes. The output is a sparse feature graph with nodes representing SAE features, error terms, and model components.

The paper evaluates circuits on subject-verb agreement variants in Pythia-70M and Gemma-2-2B. It compares feature circuits with neuron circuits obtained by the same discovery procedure. Faithfulness measures how much of the model metric the circuit explains; completeness examines whether the complement still explains behavior. Human raters judge feature interpretability and compare circuit features with random features.

The downstream editing method is Sparse Human-Interpretable Feature Trimming (SHIFT). A classifier is trained on Bias in Bios, a feature circuit explaining its predictions is discovered, and a human inspects the circuit features for task relevance. Features judged to encode unintended signals are zero-ablated; the classifier can then be retrained on the original data to recover intended performance. The important point is that the unwanted signal is identified in the model's causal feature circuit, not by requiring a separately labeled disambiguation set.

## Baselines / Comparisons
Circuit comparison uses sparse feature circuits, neuron circuits, random features, random neurons, and versions with SAE error nodes removed. For SHIFT, the paper compares the original classifier, random ablations, a neuron-based SHIFT variant, feature and neuron skylines, an oracle that knows the intended/spurious split, and a concept-based pruning (CBP) baseline.

The paper also compares SAE variants that retain all reconstruction errors with variants that remove attention/MLP error terms. This tests whether errors are merely technical noise or whether they carry information needed for faithful causal graphs.

## Experiments / Findings
On subject-verb agreement, small feature circuits explain a substantial fraction of held-out behavior. The authors report that feature circuits are more interpretable to crowdworkers than neuron circuits, and that circuit features are more interpretable than randomly sampled SAE features. Feature circuits also achieve higher faithfulness at comparable circuit sizes. Removing all SAE error nodes can reduce faithfulness, showing that reconstruction errors should not be discarded automatically.

The relative-clause case study yields small interpretable algorithms. In Pythia, a circuit with 86 nodes has faithfulness 0.21; in Gemma, a 223-node circuit reaches the same reported faithfulness. The graphs contain features that track noun number, select the correct subject across a relative clause, and compose with verb-number features. The mechanisms overlap with the simpler agreement circuit but include additional routing for the intervening clause.

SHIFT is evaluated on profession and unintended demographic labels. With the same number of ablations, random feature removal has little effect, while manually selected irrelevant features materially change the intended/spurious balance. The table reports, for example, Pythia SHIFT at 88.5 intended accuracy and 54.0 unintended accuracy, and SHIFT plus retraining at 93.1 intended and 52.0 unintended; the oracle reaches 93.0 and 49.4. Gemma SHIFT plus retraining reaches 95.0 intended and 52.4 unintended, close to the feature skyline/oracle trade-off. The exact desired direction depends on which label is treated as spurious, but the mechanism is clear: interpretable causal features make targeted editing possible.

The unsupervised pipeline clusters subcorpora by automatically discovered model behaviors and computes feature circuits for thousands of clusters. Qualitative examples include narrow induction features such as recognizing “A3 ... A” and copying the number token, which then composes with a succession feature. This demonstrates scale, while the authors explicitly treat evaluation of all automatically discovered clusters as an open problem.

## Ablation / Error Analysis
The most important ablations are error-node removal, random feature ablation, neuron replacement, and retraining after SHIFT. Error-node removal can break circuit faithfulness because SAE reconstruction residuals preserve information not captured by the sparse dictionary. Random feature ablation does not reproduce the targeted behavior, indicating that SHIFT is not just a function of removing many units.

The neuron-based SHIFT variant is less useful than feature-based SHIFT. Neurons are harder to interpret and the most causally implicated neurons do not isolate the unintended signal as cleanly. Retraining recovers performance lost when relevant features are removed, but it does not eliminate the need for correct human judgments about feature relevance.

## Limitations
Circuit quality depends on the SAE dictionary, thresholds, metric, ablation convention, and data split. Human interpretation is still required for SHIFT, and much of the large-scale evaluation is qualitative. Faithfulness and completeness are task-relative rather than a universal proof that a circuit is the model's only mechanism. The automatically discovered feature-circuit website is promising, but open-ended clusters can be hard to label and may contain multiple behaviors.

## 两句话总结
这篇论文把 SAE features 和 reconstruction-error nodes 组织成可干预的 sparse feature circuits，使机制从 polysemantic neuron 变成更细粒度的因果图。SHIFT 进一步证明，人类可以检查这些 feature circuit 并剪掉任务无关特征来改变分类器的偏置，但自动发现后的大规模语义评价仍是开放问题。

## Evidence note
This note was written from the ICLR 2025 PDF (arXiv:2403.19647v3), including the circuit-discovery algorithm, subject-verb agreement experiments, SHIFT Table 2, and the unsupervised scaling section.
