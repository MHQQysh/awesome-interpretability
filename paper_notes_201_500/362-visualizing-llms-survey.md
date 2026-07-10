# 362. Visualizing Large Language Models: A Brief Survey

- **Authors:** Adrian M. P. Brasoveanu, Arno Scharl, Lyndon J. B. Nixon, Razvan Andonie
- **Venue / year:** IEEE International Conference on Information Visualisation (IV), 2024
- **Paper:** https://doi.org/10.1109/IV64223.2024.00049
- **Tags:** `visual-analytics` `llm-interpretability` `prompt-visualization` `xai-survey`

## Introduction
LLMs are high-dimensional and their reasoning pathways are difficult to inspect, while visualization can make prompt variants, attention/activation structure, causal relations, evaluation results, and multimodal interactions navigable. This survey focuses on papers whose main contribution is a visualization system or visualization method, not papers that merely include a plot to present their own experiment.

The authors restrict the core survey to work from 2020 onward, with historically important earlier work retained in the bibliography. They aim to provide a compact map of a fast-moving area rather than a comprehensive review of every LLM paper.

## Method / Framework
The search begins with Google Scholar and DBLP to identify topics and venues, then checks major publisher portals, arXiv, code availability, and public awareness. Selection favors relevance, open access, source code, and novel or important visualization systems. Chatbot interfaces and ordinary datasets are generally excluded unless visualization is central.

The taxonomy has five broad application groups: LLM mechanisms, causality, XAI, LLM evaluation, and applications such as RAG, code generation, and multimodality. The mechanism category further covers prompt engineering, instruction tuning, guidance, visual prompt tuning, and reasoning strategies. Systems are recorded by topic and chart type, such as control panels, scatter plots, template cards, heat maps, graphs, and dashboards.

## Baselines / Comparisons
This is a taxonomy survey rather than an experimental benchmark. It contrasts visualization systems with static visual materials, visualization used only to showcase an experiment, commercial tools, and chatbot interfaces outside the scope. Examples include PromptIDE/Promptaid for prompt variation and perturbation, ChartGPT for instruction-tuned chart data, LLaVA dashboards for visual instruction tuning, and interactive knowledge-graph/causal systems.

The survey also compares visualization idioms by task: card and shopping-cart layouts for prompt search, code-block highlights for Chain-of-Code, text heat maps for explanations, node-link diagrams for causal graphs, and embedding plots or thumbnails for multimodal models. The comparison is descriptive, not a claim that one chart is universally superior.

## Experiments / Findings
The survey finds that classic line and bar charts remain common, while NLP-oriented systems frequently use text heat maps and arrows to show word connections. Prompt exploration uses cards or similar repeated-option designs; computer-vision applications use image/video grids, embeddings, and associated text heat maps. LLM mechanism and evaluation visualization are still relatively brief because those areas are young.

The paper highlights visualization's role in making causal extraction, counterfactuals, knowledge-graph evolution, prompt engineering, RAG, and multimodal reasoning inspectable. It also points to a practical trend: RAG, low-code/no-code environments, and report-generating visualizations are likely to expand because the same exploratory view can be reused in statistics, slides, and business reports.

## Ablation / Error Analysis
There is no model ablation. The survey's principal error analysis is scope and selection bias: DBLP title searches miss non-computer-science work, arXiv lacks peer review, and papers that use visualization incidentally are excluded even if they contain useful ideas. The authors also note that standard charts can become decorative rather than explanatory if they do not expose information flow or uncertainty.

## Limitations
The paper explicitly calls itself a starting point, not a comprehensive systematic review. The field changes quickly, the included systems vary in maturity and reproducibility, and selection favors open access, code, and visible work. Many visualization systems are described at a high level, so there is no unified user-performance or explanation-faithfulness metric.

## 两句话总结
这篇 survey 按“机制、因果、XAI、评测和应用”整理了 LLM 可视化系统，覆盖 prompt 变体、内部路径、知识图谱、RAG 和多模态交互等视图。它的价值在于说明不同图形承担不同解释任务，但由于研究很新且筛选偏向公开代码和系统论文，不能当作完整的定量排行榜。

## Evidence note
本笔记依据 IEEE IV 2024 作者稿全文逐节核对；论文强调的是 visualization systems/methods 的范围，未报告统一的受试者实验或模型性能数字。
