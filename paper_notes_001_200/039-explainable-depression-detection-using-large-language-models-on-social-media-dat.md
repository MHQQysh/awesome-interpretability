# 039. Explainable Depression Detection Using Large Language Models on Social Media Data

- **作者 / venue**：Nour K. et al.；CLPsych 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.clpsych-1.8/)
- **任务**：从社交媒体文本检索抑郁相关证据，并填充 Beck Depression Inventory（BDI）

## 1. Introduction

社交媒体包含大量个人表达，但直接把所有帖子送入模型会混入无关内容、噪声和隐私风险。抑郁检测尤其需要解释：系统应指出哪些文字对应哪些症状，而不是只输出一个风险标签。

## 2. Method

- 按 BDI 的 21 个症状构造 symptom-related writing retrieval。
- 用句子 embedding/USESim 等方法从用户帖子中选 top-1/top-5 relevant writings。
- 将选出的证据填入 BDI questionnaire，再用 LLM 进行症状估计。
- 通过检索、症状填充和最终 depression estimation 分阶段评价，形成可追溯证据链。

## 3. Baseline 与对比

- 直接使用全部社交媒体文本的方法。
- Skaik and Inkpen 等 topic-based filtering + estimation 方法。
- top-1 vs top-5 retrieval。
- 不同句子 embedding/检索模型，以及 LLM-based estimation。
- 指标包括 ACR、ADODL、DCHR 等与 BDI/症状估计相关的任务指标。

## 4. Findings

- 先检索症状相关句子能清理噪声，使 LLM 更容易围绕 BDI 具体问题作答。
- 最佳系统在四个主要指标中的三个取得最好结果，说明 explanation-oriented evidence selection 不只是生成一段理由，也能改善下游估计。
- top-5 证据通常带来更完整上下文，但会增加无关文本和 token 成本；top-1 更紧凑，可能漏掉症状证据。
- 论文的可解释性来自“哪几条用户文字支持哪一个 BDI item”，而非 LLM 自由生成的心理诊断理由。

## 5. 局限与伦理

研究没有精神健康专业人员参与全部评估；社交媒体文本不等于临床诊断。数据隐私、误报、用户同意和危机干预必须由专业系统处理，不能把模型输出当作医疗结论。

**一句话评价**：论文把 explainability 设计成 evidence retrieval + symptom mapping 流程，避免让 LLM 对长篇社交媒体内容直接给出不可追溯的抑郁判断。
