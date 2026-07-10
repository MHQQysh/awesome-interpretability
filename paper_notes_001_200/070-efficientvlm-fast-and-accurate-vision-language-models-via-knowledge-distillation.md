# 070. EfficientVLM: Fast and Accurate Vision-Language Models via Knowledge Distillation and Modal-adaptive Pruning

- **作者 / venue**：Findings of ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.findings-acl.873/)

## Introduction / Method

VLM 参数和视觉/文本 encoder 成本高，但不同下游任务对两种模态依赖不同。EfficientVLM 用 knowledge distillation 保持 teacher 能力，再按 modal-specific importance 对 visual/text encoder 做自适应 pruning。

## Baseline

比较 MiniVLM、DistilVLM、KD-only、pruning-only 和 manually tuned sparsity。任务覆盖 vision-language understanding，检查不同任务需要保留多少视觉/语言容量。

## Findings

- 不同任务的 modal importance 差异明显，统一 sparsity 会浪费容量。
- modal-adaptive pruning + KD 达到接近 teacher 的效果，论文报告保留约 **98.4%** performance。
- 只 pruning 不 distillation 会明显下降；KD-only 与 EfficientVLM 相近时，说明需要区分速度收益和准确率收益。
- pruning pattern 也提供一种解释：模型在具体任务中更依赖视觉还是文本路径。

## 局限

importance estimation 依赖任务和数据；剪枝后保留的模块不一定是因果解释。部署还需评估真实 latency、显存和量化兼容性。

**一句话评价**：EfficientVLM 将模态依赖分析转为可操作的 pruning budget，为多模态模型的效率和结构解释提供共同接口。
