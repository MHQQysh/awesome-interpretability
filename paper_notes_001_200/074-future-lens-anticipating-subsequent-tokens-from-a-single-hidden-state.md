# 074. Future Lens: Anticipating Subsequent Tokens from a Single Hidden State

- **作者 / venue**：CoNLL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.conll-1.37/)

## Introduction / Method

logit lens 通常预测下一个 token；Future Lens 研究单个 input-token hidden state 是否包含更远未来 token 的信息。方法训练/学习 prompt 或 projection，使单个 hidden state 能预测 subsequent token sequence，并比较不同 layer 的信息量。

## Baseline 与对比

比较标准 logit lens、tuned lens、固定 generic prompt 和直接运行完整模型；指标为 precision@1/5/10 与未来 token 数量。

## Findings

某些 layer 的 hidden state 已包含超出 next token 的未来信息。优化到 N=1 的 prompt 平均比最佳 baseline 提高 precision@1 约 **24.8%**、precision@5 约 **25.3%**、precision@10 约 **25.1%**。这说明 Transformer 在中间状态中提前构建了部分未来预测。

## 局限

未来预测受 prompt、短语分布和层位置影响；“能预测”不代表模型已经明确计算了完整未来计划。需要 causal patching 区分信息存在和信息使用。

**一句话评价**：Future Lens 将 lens 从“读出当前预测”扩展到“读出隐藏状态中的未来 token 轨迹”。
