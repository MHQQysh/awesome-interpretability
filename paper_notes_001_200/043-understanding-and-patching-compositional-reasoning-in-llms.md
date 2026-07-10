# 043. Understanding and Patching Compositional Reasoning in LLMs

- **作者 / venue**：Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.576/)
- **任务**：定位 compositional reasoning failure，并用 CREME 做局部修补

## Introduction

组合推理要求模型把多个事实或关系按正确顺序组合，但 LLM 常走 shortcut：直接把问题中的表面词和答案绑定，跳过隐式中间结论。最终错误不是简单的 knowledge absence，而可能是中间表示没有被正确生成或使用。

## Method

- 用 logit lens 检查 middle layers 是否已经出现隐式 reasoning result。
- 对 compositional query 和 corresponding second-hop query 做对比。
- 分析错误样本中的 hidden-state trajectory，寻找 causal layer/component。
- 提出 CREME，通过 intervention/editing 修改相关 MHSA output matrix 或 hidden representation，使中间推理结果被正确利用。

## Baseline 与对比

- 普通 LLM prompting/CoT。
- 其他 compositional reasoning correction 方法。
- 经典 model editing baseline（如 ROME 类方法）。
- paraphrased input 和 random editing direction controls。

## Findings

- 很多错误模型内部其实已经产生了正确的隐式中间结果，但后续层没有正确读取或组合它。
- logit lens 能在中间层看到这些结果，activation intervention 进一步证明它们对最终输出具有因果作用。
- CREME 修补后，组合推理任务性能提升；随机编辑或过强编辑会破坏其他能力。
- 论文把 shortcut reasoning 从输出错误追溯到“中间结果生成/使用断裂”，比单纯 prompt 改写更接近机制解释。

## 局限

编辑一个任务的 MHSA/output matrix 可能影响其他事实和关系；logit lens 对中间表示的解释仍受 vocabulary projection 限制。需要跨任务、跨模型和更多 compositional depth 的复现。

**一句话评价**：CREME 的关键证据链是“中间层出现正确结果 -> 干预该表示改变行为”，说明组合推理失败可能是利用错误而不是知识缺失。
