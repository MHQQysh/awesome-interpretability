# 361. Towards More Trustworthy and Interpretable LLMs for Code through Syntax-Grounded Explanations

- **Authors:** David N. Palacio, Daniel Rodriguez-Cardenas, Alejandro Velasco, Dipin Khati, Kevin Moran, Denys Poshyvanyk
- **Venue / year:** ACM Transactions on Software Engineering and Methodology manuscript, 2024
- **Paper:** https://arxiv.org/abs/2407.08983
- **Tags:** `code-llm` `syntax-grounded` `confidence` `human-study` `causal-interpretability`

## Introduction
Code LLMs are usually summarized by aggregate accuracy, BLEU, CodeBLEU, or pass rates, but these metrics hide which part of a generated program is uncertain or wrong. The paper argues that trust requires a concept a programmer recognizes at prediction time, and proposes to ground confidence in Abstract Syntax Tree (AST) categories rather than leaving it at token level.

ASTrust answers both local and global questions: which syntax category in this snippet is low confidence, and which categories are difficult for a model across a code corpus? The method is post-hoc and does not claim to recover the internal algorithm; it provides a structural map from model confidence to familiar programming concepts.

## Method / Framework
ASTrust parses code into AST syntax categories and aggregates next-token confidence over tokens belonging to each category. It uses an alignment/clustering formalism to map token-level probabilities to categories and subcategories, then visualizes local explanations as sequences, heat maps, and AST graphs, and global explanations as category-level heat maps.

The evaluation contains 12 code LLMs and code from recent commits of the 200 most popular Python GitHub projects. A 0.6 confidence threshold is used for the interpretability analyses. The paper also uses a causal inference design to test whether higher ASTrust confidence is associated with lower cross-entropy learning error, and a human survey with 27 participants who inspect GPT-3 completions under different explanation visualizations.

## Baselines / Comparisons
The conceptual baselines are canonical intrinsic metrics, extrinsic BLEU-4/CodeBLEU, token-level confidence, and a sequence explanation without AST structure. The human study compares AST-based visualizations with sequence and other local forms, including AST variants with different complexity.

The model comparison spans GPT-3-type, code-generation, mono-language, and multi-language models from 110M to 2.7B parameters. This is important because ASTrust can reveal category-level differences that an aggregate score misses: a smaller model can have a better CodeBLEU while being much less reliable on particular syntax categories.

## Experiments / Findings
In the human study, 67.48% of participants judged ASTrust information useful or slightly useful, and 75% judged the graph-style local explanation useful. AST visualizations were preferred for assessing reliability, although complete ASTs could be more complex. Participants liked the color coding and confidence values, while some found whitespace/punctuation distracting and requested collapsible AST nodes.

In the 12-model data study, Iteration, Exception, and Scope generally exceed the 0.6 confidence threshold, while Natural Language and Data Types are difficult. The largest mono-language model reaches an average intrinsic accuracy of 0.84 and exceeds the threshold on every syntax category. ASTrust adds information beyond CodeBLEU: for example, a small model can have CodeBLEU 0.194 while showing weak Natural Language and Data Types confidence.

The causal analysis finds that ASTrust probabilities tend to negatively affect cross-entropy loss, so better syntax-category prediction corresponds to lower learning error. Natural Language identifiers have particularly large negative effects, while some Functional Programming categories, such as lambda, have weak or positive effects for natural-language-oriented models.

## Ablation / Error Analysis
The method comparison is primarily a visualization ablation: sequence-only, AST sequence, and graph/AST representations. Full AST views can overload users, while simpler AST variants are easier to read. The model-scale corner case shows that Data Types and Natural Language are especially sensitive to context length, token position, and training distribution.

ASTrust itself is also a diagnostic check. If logits are poorly predicted because of overfitting or underfitting, the causal effect on learning error weakens. The authors caution that syntax categories can be structurally valid yet semantically wrong, so a confidence map should not be mistaken for a proof that generated code is correct.

## Limitations
The approach depends on accessible logits and a parser, so it is harder to use with closed API models or invalid code. The 0.6 threshold is tunable, the syntax mapping is a post-hoc proxy, and the human sample is small. Syntax alone cannot capture all semantic and security properties, while some categories require larger context than the snippet provides.

## 两句话总结
ASTrust 将代码 LLM 的 token 置信度聚合到 AST 的数据结构、循环、作用域和操作符等语法类别，用序列、热图和 AST 图把“哪里不可靠”变成程序员能理解的概念。12 个模型和 27 人研究显示这种解释比单一准确率更能定位错误类型，但它仍是置信度代理，不能替代语义、执行和安全验证。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；原 PDF 的 ACM DOI 仍为占位符，本文按可获得的作者稿和表 2/表 3 记录结果。
