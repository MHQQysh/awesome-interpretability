# 250. Paths-over-Graph: Knowledge Graph Empowered Large Language Model Reasoning

- **Authors:** Paper metadata lists a WWW 2025 contribution
- **Venue:** WWW 2025
- **Paper:** https://doi.org/10.1145/3696410.3714892
- **Tags:** Knowledge Graph, Multi-Hop Reasoning, Path Retrieval, LLM

## Introduction

LLM 在多实体、多跳问题上容易被知识图的大量无关邻居和关系噪声淹没；只把 subgraph 文本化仍不能保证找到连接多个实体的深层路径。Paths-over-Graph (PoG) 的问题是能否利用 graph structure prune irrelevant noise，并把可读 reasoning paths 交给 LLM。

## Method / Framework

PoG 从 query entities 出发探索/筛选多跳 paths，利用图拓扑和语义相关性压缩候选，再将路径组织成 LLM 可读的 reasoning context；LLM 基于 path understanding 生成答案。核心不是把整个 graph 交给模型，而是显式搜索能连接实体和关系的 path。

## Baselines / Comparisons

应与 closed-book LLM、text RAG、subgraph GraphRAG、KGQA/path-based retrieval 和其他 LLM-KG reasoning 方法比较，指标包括 multi-hop QA accuracy、答案一致性、hop/path recall、噪声敏感性和推理成本。

## Experiments / Findings

- PoG 的 path pruning 减少无关节点，让 LLM 能处理 multi-entity deep paths；其贡献是把 graph structure 作为推理控制而非静态 context。
- 路径本身提供可追溯 rationale：用户可以检查实体、关系和 hop 顺序，定位是检索错还是 LLM 组合错。
- 正式表格和数据应以 WWW 版本核对；本笔记不把二手摘要中的 SOTA 数字当作已验证结果。

## Ablation / Error Analysis

关键消融是 path depth、entity linking、graph pruning、path ranking 和 LLM context format。路径过短会漏掉答案，过长会重新引入噪声；实体链接错、图缺边、关系方向错误和 LLM 忽略中间 hop 是主要失败。

## Limitations

PoG 依赖高覆盖、高质量知识图和准确实体链接，开放世界新知识仍受限；path correctness 不保证最终答案的事实性。多跳搜索成本和图投毒/过时知识也会影响可靠性。

## 两句话总结

PoG 用图路径搜索代替把整个知识图塞给 LLM，显式剪枝无关邻居并保留多实体深层推理轨迹。它提升可追溯多跳 reasoning 的方向是清晰的，但图覆盖、路径错误和正式实验细节仍需核验。
