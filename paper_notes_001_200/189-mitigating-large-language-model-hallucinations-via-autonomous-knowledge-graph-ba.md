# 189. Mitigating Large Language Model Hallucinations via Autonomous Knowledge Graph-Based Retrofitting

- **Authors:** Xinyan Guan, Yanjiang Liu, Hongyu Lin, Yaojie Lu, Ben He, Xianpei Han, Le Sun
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/29770
- **Tags:** Hallucination, Knowledge Graph, Retrofitting, Verification

## Introduction

已有 KG-RAG 通常只用用户 query 中显式实体检索，无法验证 LLM 在多步 reasoning 中自行生成的中间事实。KGR 要在初稿之后提取、选择、验证和 retrofit factual states，自动修复 reasoning 过程中出现的错误。

## Method / Framework

LLM 先生成 draft；系统抽取其中 factual statements，识别实体关系，并从 KG 获取候选 triples；再对候选做 selection / validation，把可信事实替换回 draft，形成 autonomous knowledge verifying and refining loop。

## Baselines / Comparisons

比较 closed-book LLM、传统 query-only KG retrieval、文本 RAG、KG context augmentation 和其它 hallucination mitigation。指标包括 factual QA accuracy、复杂推理、实体 / 日期正确性、人工 faithfulness 与生成质量。

## Experiments / Findings

- KGR 在 factual QA benchmark 上显著提升，尤其是需要复杂多步 reasoning 的问题。
- retrofit 中间 states 比只检索 query entity 更能纠正模型自己生成的错误事实，例如日期和关系。
- 自动 extraction-selection-validation 让系统无需逐条人工编辑，但 KG coverage 和 relation matching 决定收益。

## Ablation / Error Analysis

消融 extraction、KG retrieval、validation、retrofit 顺序和多跳深度。错误来自抽取漏掉事实、KG 缺边、实体链接错误以及把模型推断而非事实陈述强行 retrofit；过多 triples 会增加上下文噪声。

## Limitations

KG 不完整或过时会导致 false correction，开放域新事实覆盖不足。LLM 仍参与抽取和验证，不能完全排除 hallucination；多轮 retrofitting 增加成本和延迟。

## 两句话总结

KGR 不只给 query 查图，而是把 LLM 初稿中的中间事实也抽出来用 KG 验证和回写。它改善复杂 factual reasoning 的可靠性，但图覆盖、实体链接和自动验证误差仍是系统瓶颈。
