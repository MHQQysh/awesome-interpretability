# 068. SCOTT: Self-Consistent Chain-of-Thought Distillation

- **作者 / venue**：ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-long.304/)

## Introduction / Method

CoT rationale 可能幻觉或与模型决策不一致。SCOTT 从大 teacher 蒸馏小 student，但通过 contrastive decoding 生成支持 gold answer 的多条 rationale，并训练 student 在 rationale 变化时保持 self-consistency。

## Baseline

比较标准 knowledge distillation、teacher-generated CoT、普通 CoT fine-tuning 和不同 rationale sampling。teacher 使用 GPT-NeoX-20B，以便获取 token-level probabilities。

## Findings

contrastive rationale 能减少 teacher 解释中的无关/错误信息；student 不仅取得任务性能，还更愿意依据其 rationale 做出对应 decision。论文分析显示 rationale refinement 可以提升一致性，但不意味着所有文字都是真正的内部推理。

## 局限

teacher quality、contrastive prompt 和 gold answer 依赖会影响蒸馏；小模型可能学到格式而非 causal reasoning。需要独立 verifier 检查 rationale 的事实和必要性。

**一句话评价**：SCOTT 将“答案蒸馏”与“解释自洽性蒸馏”结合，试图让小模型的 CoT 更接近其真实决策路径。
