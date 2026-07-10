# 125. Evaluating LLMs for Targeted Concept Simplification for Domain-Specific Texts

> 人工精读笔记：EMNLP 2024。重点是简化指定难概念而不是把整段文本变短，并分别测 meaning preservation、理解度和可读性。

## 0. 论文信息

- **作者**：Sumit Asthana, Hannah 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.357)
- **资源**：[WIKIDOMAINS](https://github.com/google-deepmind/wikidomains)
- **领域**：CS、economics、biology 等 domain-specific definitions

## 1. Introduction：要解决什么问题

普通 text simplification 往往降低整段句法复杂度，却不解决读者真正不懂的专业 concept。对 domain text，增加一个上下文解释可能比替换一个难词更有帮助；但解释过多会改变原意或造成新的事实错误。

## 2. Method / Framework：怎么解决

作者提出 targeted concept simplification：给定含一个 difficult concept 的定义，只改写与该 concept 相关的内容。WIKIDOMAINS 从 Wikipedia domain definitions 构建数据，并用 specificity/概念熟悉度 heuristic 选择 target。

比较三类生成：dictionary lookup 后追加定义、simplify 目标概念、explain 目标概念并保留上下文。人工从 meaning preservation、陌生读者是否理解、文本是否更易读三个维度评分。

## 3. Baseline / 对比基线

dictionary definition append baseline；LLM lexical simplification；LLM targeted explanation；GPT-4、PaLM-2、Falcon-40B 等模型；BLEU/ROUGE、Flesch、AoA、concept difficulty 等自动指标。

## 4. Experiments / Findings：结果如何读

explanation/contextual elaboration 往往比简单替换 difficult concept 更能帮助不熟悉概念的读者，同时更好保持 meaning。dictionary baseline 在部分 meaning preservation 上稳健，但容易打断语篇、追加过长定义。

自动 simplification metrics 与 human understanding/meaning preservation 的相关性只有约 0.2 量级，甚至常规 Flesch reading ease 也无法判断目标概念是否真的被解释清楚。LLM 可能过度改写整句、引入无关细节或把概念含义简化错误。

## 5. Ablation / 机制解释

definition/simplify/explain 三种 prompt、dictionary vs LLM、zero-shot vs few-shot、不同 domain 和 difficulty 构成主要消融。结果支持“针对概念 + 提供上下文”比纯 lexical simplification 更符合读者需求。

## 6. Limitations / 局限与复现注意

concept difficulty 个体化很强，Wikipedia domain 有选择偏差；三位 annotators 的主观理解会影响结果；自动指标不应替代读者研究；解释可能引入新的事实/安全错误。

## 7. 两句话总结

论文提出 targeted concept simplification，专门解释读者不懂的 domain concept，而不是粗暴压低整段文本难度。人工结果偏好 contextual explanation，但常用 BLEU/Flesch 等指标与真实理解度相关性弱，说明概念级简化需要专门评测。
