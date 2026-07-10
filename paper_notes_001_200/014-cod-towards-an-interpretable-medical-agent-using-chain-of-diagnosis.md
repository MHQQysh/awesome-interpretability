# 014. CoD, Towards an Interpretable Medical Agent using Chain of Diagnosis

- **作者 / venue**：Junying Chen, Chi Gui, Anningzhe Gao, Ke Ji, Xidong Wang, Xiang Wan, Benyou Wang；Findings of ACL 2025
- **论文**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.740/)
- **任务**：医疗自动诊断；将诊断过程拆成症状抽象、候选疾病、追问、推理、置信度和决策

## 1. Introduction：从黑盒诊断到可检查的诊断链

医疗诊断不只是输出一个疾病名称。医生会先整理主诉，再回忆候选疾病，询问能减少不确定性的症状，最后根据证据和置信度作出决定。LLM 的直接诊断虽然有语言和知识优势，但常常问错问题、过早下结论，用户也看不到它为什么相信某个疾病。

CoD 的切入点是把诊断过程改造成显式链条，并让每一步产生可读中间状态。作者还提出 DxBench，扩大自动诊断评测中的疾病和症状覆盖，避免只在小规模病例上展示可解释性。

## 2. Method：Chain-of-Diagnosis 的五步

1. **Symptom abstraction**：把患者叙述整理成显式/隐式症状，减少自然语言表面差异。
2. **Candidate disease recall**：根据症状从疾病百科或知识库召回候选集合，而不是在全疾病空间中自由生成。
3. **Diagnostic reasoning and inquiry**：比较候选疾病，选择能最大幅度减少不确定性的后续问题。
4. **Confidence assessment**：输出候选疾病的 confidence distribution，并用熵衡量当前不确定性。
5. **Decision making**：当最高置信度超过阈值时作出诊断，否则继续询问或保持不确定。

作者据此训练 DiagnosisGPT。置信度不是事后解释文本，而是直接参与控制：阈值 τ 决定是否停止，置信度分布还可以被用户/系统读取。

## 3. Baseline 与对比

- **传统自动诊断**：监督式模型、规则/知识库方法，在固定症状和疾病空间中作比较。
- **LLM baseline**：GPT-4、Gemini-Pro、ERNIE Bot、Claude-3-Opus 等，既比较直接诊断，也比较带 symptom inquiry 的流程。
- **流程 baseline**：DiagnosisGPT_baseline，即使用 CoD 训练数据但移除关键 confidence/决策机制的版本。
- **数据集**：Dxy、MIMIC 等真实或半结构化诊断数据，以及作者构建的 DxBench（9,604 种疾病、5,038 种症状、15 个科室）。
- **指标**：accuracy、询问轮数、confidence 与正确率的一致性、诊断链完整度、熵下降以及不同阈值 τ 下的控制效果。

## 4. Findings：置信度是否真的有用

- 在开放式诊断中，DiagnosisGPT 随着可用症状和询问轮数增加而提高准确率；在 DxBench 上，τ=0.6 的设置达到约 **44.2%**，高于较低阈值设置。
- 在自动诊断 benchmark 上，置信度阈值不仅影响准确率，也影响需要的询问次数。τ 从 0.4 提升到 0.6 时，模型更谨慎，准确率和平均询问成本同时上升。
- 论文报告在多个数据集上，当 τ≈0.55 时可以达到 **90% 以上**的高置信度诊断准确率；这不是全量问题的 accuracy，而是“模型足够自信的子集”的可靠性。
- 熵分析显示，正确的 symptom inquiry 通常降低候选疾病分布的熵；这给出了“为什么问这个问题”的过程性解释，而不是只显示最后一条 CoT。

## 5. Ablation 与可解释性分析

- **w/o Confidence for Decision**：模型失去置信度控制后，行为更接近普通 LLM，诊断准确率和停止逻辑下降。
- **DiagnosisGPT_baseline**：移除 CoD 的结构化决策链后，基准表中表现低于完整 CoD，说明 gains 不只是来自额外训练数据。
- 对比 Chain-of-Thought，CoD 的优势不是文字更长，而是每一步绑定了症状、候选疾病、置信度和决策条件；完整度评估显示诊断链比普通推理更能覆盖诊断过程。

## 6. 局限与医疗风险

DxBench 主要来自结构化疾病百科和生成/整理的病例，不能替代真实临床验证；诊断正确率也不能证明系统安全。confidence 可能校准得很好但仍基于错误知识，阈值控制只能管理不确定性，不能消除医疗幻觉。

**一句话评价**：CoD 将“可解释医疗 agent”具体化为可中止、可追问、可检查置信度的诊断流程；最有价值的不是给出更长解释，而是让内部不确定性影响下一步行动。
