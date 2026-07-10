# 092. Refining Attention for Explainable and Noise-Robust Fact-Checking with Transformers

> 人工精读笔记：EMNLP 2025。重点是把 evidence explanation 放回 verifier 的训练回路，而不是训练一个与决策无关的后处理 explainer。

## 0. 论文信息

- **作者**：Jean-Flavien Bussotti, Paolo Papotti
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.1295)
- **代码**：[ATTUN](https://github.com/JeFlBu/ATTUN)
- **数据**：FEVEROUS、AVeriTeC、FM2、SciFact；另有 QA 实验

## 1. Introduction：要解决什么问题

开放域 fact-checking 的 retriever 往往给 verifier 20--40 个 passage，其中既有支持/反驳 claim 的证据，也有同一实体的噪声信息。传统 transformer 能做 Supports/Refutes/NEI 分类，却不保证模型真正使用了可展示的证据；单独训练 evidence ranker、SHAP/LIME 或让 LLM 事后解释，可能解释的是另一个模型的判断。

作者提出 ATTUN，直接从 verifier 的 self-attention 计算 evidence usefulness，并用该 usefulness loss 反过来调整 attention。目标同时包括更准确的 claim classification、更抗噪声的证据选择，以及与内部决策一致的解释。

## 2. Method / Framework：怎么解决

对每个 evidence span，ATTUN 计算各 layer/head 中 evidence token 与整个输入的平均 attention，再除以同一 head 对 claim span 的 reference attention，得到 evidence-specific attention ratio。展平后进入线性 classifier，预测该 evidence 是否有用。

Evidence classifier 的 binary cross-entropy 与原始 verifier loss 相加：总损失为 model loss + gamma × evidence loss。由于 explainer 在 fine-tuning 时和 verifier 共享 attention，模型被鼓励把可用于决策的权重放到真正 evidence，而不是事后猜理由。

ATTUN 也提供每个 label 独立 classifier，允许 Supports/Refutes/NEI 使用不同证据关注模式。ATTEX 是不与 verifier 联合训练的简化 attention-analysis baseline，用来检验共享学习是否重要。

## 3. Baseline / 对比基线

- **GFCE**：带内生解释的 fact-checking 方法。
- **LLaMA3、GPT-4o mini**：prompt 直接先找 evidence 再分类，其中 GPT-4o mini 还有 fine-tuned 版本。
- **RoBERTa、DeBERTa-V3、Phi-4 mini + SHAP**：后处理 attribution baseline。
- **ATTEX**：verifier 与 explainer 独立训练，测试 joint training 的价值。
- **不同 retriever recall**：检验方法在 gold evidence 缺失和完整的 noisy context 下是否仍有效。

## 4. Experiments / Findings：结果如何读

ATTUN 在四个 fact-checking 数据集上通常取得最高 evidence identification。Feverous 的 F1 Useful 从约 0.49 提升到 0.74，论文将其概括为最高约 51% 提升；该数据集 retriever recall 只有 0.36，说明 ATTUN 可以在不完整上下文中尽量从已有 passage 找有用证据。

在 gold evidence recall=1 的数据集上，ATTUN 仍带来 verifier accuracy 和 evidence F1 提升，而不是只受益于缺失证据场景。表中例如 RoBERTa+ATTUN 在 Feverous accuracy/F1 约 0.68/0.75、F1 Useful 约 0.73；DeBERTa+ATTUN 约 0.72/0.78、F1 Useful 约 0.74。相比同 backbone 的 SHAP/ATTEX，joint ATTUN 的解释质量和 Noise F1 更稳。

论文还报告任务 accuracy 最高约 18% 的提升；QA 附录显示将注意力 refinement 用于问答也有收益。这里应把 explanation 指标和 verifier 指标分开看：ATTUN 的贡献是同时提升两者，而不是只提高证据召回。

## 5. Ablation / 机制解释

- **ATTUN vs ATTEX**：共享 attention refinement 的版本更好，支持解释和分类共同训练。
- **ATTUN vs SHAP**：后处理 SHAP 计算昂贵，且不改变 verifier 的噪声鲁棒性；ATTUN 把 attribution 作为训练信号。
- **单 classifier vs label-specific classifier**：不同 claim label 需要的 evidence 类型不同，label-specific 设计更灵活。
- **Noisy/incomplete evidence**：在 recall<1 和 recall=1 的数据分别测试，确认收益不是单一 retriever artifact。

## 6. Limitations / 局限与复现注意

- Attention 不是天然忠实解释；ATTUN 通过联合监督增强可用性，但仍不等于完整 causal faithfulness。
- Evidence span、reference claim span 和 gamma 的设计会显著影响解释标签。
- 如果 retriever 完全没有关键事实，attention refinement 无法凭空恢复证据。
- 论文主要在短文本 fact-checking/QA 上验证，长上下文、多跳证据链和开放生成仍需测试。

## 7. 两句话总结

ATTUN 解决 fact-checking 中 verifier 对噪声敏感、后处理解释与真实决策脱节的问题。它用 attention ratio 预测 evidence usefulness，并把 evidence loss 与分类 loss 联合训练，在四个数据集上同时提升证据识别和任务准确率，但 attention 的因果忠实性仍需要额外干预验证。
