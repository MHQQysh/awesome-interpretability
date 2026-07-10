# 022. LLMCheckup: Conversational Examination of Large Language Models via Interpretability Tools and Self-Explanations

- **作者 / venue**：Qianli Wang, Tatiana Anikina, Nils Feldhus, Josef van Genabith, Leonhard Hennig, Sebastian Möller；HCINLP 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.hcinlp-1.9/)
- **任务**：交互式解释工具；通过对话组合 attribution、可视化、下游预测和 LLM self-explanation

## 1. Introduction

一次性的 feature attribution 或 rationale 往往不能回答用户的追问：为什么这个 token 重要？换一个样本还成立吗？如果用户不懂 interpretability 工具，单独提供一张图也很难使用。LLMCheckup 把解释过程设计成对话，让用户可以从单个实例追问到方法、特征、模型比较和错误分析。

## 2. System / Method

- **Parsing**：解析不同数据类型和任务的输入。
- **Downstream prediction**：展示模型预测以及可供解释的输出。
- **White-box tools**：集成 feature attribution、单样本可视化和模型内部分析。
- **Self-explanation**：用 autoregressive LLM 生成 rationale，并允许用户询问理由。
- **Dialogue control**：系统根据用户问题调用相应操作，而不是固定执行一种 XAI 方法。

论文还比较两种 parsing approach：GD 与 MP；解析准确率随模型规模增长，说明对话式工具的输入理解本身也是系统瓶颈。

## 3. Baseline 与对比

- one-off explanation 与 conversational explanation：前者提供一次结果，后者允许追问。
- Nearest-neighbor baseline：使用语义相似度寻找类似样本，作为解析/示例检索的简单对照。
- GD/MP parsing approaches：比较两种任务和数据解析流程。
- 不同语言模型：主要看模型大小对解析、解释和下游任务的影响，不以追求单项 state-of-the-art 为目标。

## 4. Findings

- 对话式解释能把 attribution、可视化和 self-explanation 组织成连续检查流程，用户不必手动切换工具。
- LLM 生成的解释适合回答“为什么”类自然语言问题，但系统仍需把解释和 white-box evidence 分开显示，避免把 rationale 当作内部因果证明。
- parsing accuracy 会影响后续所有模块：任务类型、输入格式和预测对象解析错误时，后续 explanation 可能看似流畅但指向错误实例。
- 系统更适合模型比较、错误分析和教学式检查，不是一个自动证明解释忠实性的评测器。

## 5. 局限与复现

工具依赖具体任务适配和外部模块，迁移到新任务需要改代码；对话式系统还会引入 LLM 自己的幻觉和解释一致性问题。复现时要固定解析器、解释工具、LLM、prompt 和对话历史。

**一句话评价**：LLMCheckup 的贡献是把解释从静态产物变成可追问的 inspection workflow，但白盒 attribution 和 LLM self-explanation 必须分层呈现。
