# 031. Steering Llama 2 via Contrastive Activation Addition

- **作者 / venue**：Nina Rimsky, Nate Gillman, Nicholas J. Ward, David Bau；ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.828/)
- **任务**：不改参数地 steering Llama 2 的行为

## 1. Introduction

RLHF 和 instruction tuning 能改变模型行为，但训练成本高且不一定能针对单一行为精准控制。论文关注 representation engineering：模型的抽象概念在 residual stream 中可能近似线性编码，因此可以从正负行为样本的激活差异中得到 steering vector。

## 2. Method：Contrastive Activation Addition（CAA）

对一组 positive/negative prompt 分别做 forward pass，取对应 residual stream activation 的平均差，得到 steering vector。推理时把向量加到指定层的 residual stream，控制模型倾向于拒答、诚实、无害或其他目标行为。与单个示例向量相比，contrastive averaging 能减少 prompt 偶然性。

## 3. Baseline 与对比

- 原始 Llama 2 / prompting baseline。
- Activation Addition：单对激活差异的早期 steering 方法。
- supervised fine-tuning：用行为数据改参数，但不专门优化 hyperparameter 以追求绝对最优。
- 不同层、向量强度、token position 的 steering 对照。

## 4. Findings

- CAA 能以较低计算成本改变目标行为，不需重新训练模型。
- 正负行为差异在 residual stream 中表现为可复用方向，支持抽象行为具有线性 representation 的假设。
- steering 强度过大时会损伤语言质量和其他能力；在 prompt 后所有 token 位置施加向量虽然简单，但会限制可用强度。
- 层位置很关键：不同层的向量对行为控制和副作用不同，不能把 steering vector 当作模型无关的全局方向。

## 5. 局限

向量可能同时携带风格、主题和长度等混杂因素；行为被改变不等于内部机制被解释。对抗 prompt、跨模型迁移、长期安全和误用风险仍需评估。

**一句话评价**：CAA 把 activation difference 变成可操作的行为控制接口，是从“发现表征”走向“无参数 steering”的代表工作。
