# 186. Enhancing Chain of Thought Prompting in Large Language Models via Reasoning Patterns

- **Authors:** Yufeng Zhang, Xuepeng Wang, Lingxiang Wu, Jinqiao Wang
- **Venue:** AAAI 2025
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/34793
- **Tags:** CoT, Reasoning Pattern, Prompt Selection, Interpretability

## Introduction

无监督 CoT selection 常按问题语义相似挑 exemplar，可能选到题面相似但推理方式不同的样本，也难解释为什么该示例有用。作者用 reasoning pattern 表示从问题到答案的过程，以结构相似而非表面相似选择 demonstrations。

## Method / Framework

先用大模型和 prompt 构造 task-specific reasoning pattern set，再按 pattern 选择多样 exemplars，组成 CoT prompt。pattern 提供选择依据，也作为人可读的解释标签。

## Baselines / Comparisons

比较 random、semantic similarity、Manual-CoT、Auto-CoT、self-consistency 和其它 prompt selection，在 arithmetic、commonsense、symbolic 等 reasoning tasks 上测 accuracy、稳定性和 pattern coverage。

## Experiments / Findings

- pattern-based selection 在多个任务上稳定提升，降低语义相似但推理不匹配的 noise。
- 多样 pattern 比重复同一种 chain 更有效，尤其是需要不同分解、反证或组合策略的题。
- pattern 标签让 prompt 构造过程更可审计，能分析模型使用的 reasoning family。

## Ablation / Error Analysis

消融 pattern 数、pattern clustering、diversity、semantic filter 和 exemplar order。pattern 生成错误会把不同策略合并或把同一策略拆开；pattern 过细导致候选稀疏，过粗又失去选择价值。

## Limitations

pattern 依赖 LLM 自动归纳，可能带入错误的解释标签；选择收益受模型、任务和 prompt budget 影响。pattern similarity 仍是 proxy，不证明模型真的执行了该 pattern。

## 两句话总结

论文用 reasoning pattern 取代单纯题面相似度来选 CoT exemplars，兼顾多样性、鲁棒性和选择可解释性。它改善多种 reasoning task，但 pattern 本身仍需人工或程序验证。
