# 030. Mitigating Hallucinations in Large Vision-Language Models with Instruction Contrastive Decoding

- **作者 / venue**：Xintong Wang, Jingheng Pan, Ding Liang, Chris Biemann；Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.937/)
- **任务**：LVLM object/attribute/relation hallucination；推理时用 instruction contrastive decoding 抑制错误概念

## 1. Introduction

LVLM 会生成视觉上不存在的对象、错误属性或错误关系。问题不仅来自语言模型本身，也来自视觉-语言 fusion module：不同 instruction 可能激活模型的幻觉概念，尤其当训练数据中对象共现分布不平衡时，模型会把“常见搭配”当成当前图像证据。

论文观察到 disturbance instruction 会放大 hallucination，因此可以把它作为对照分布，用 contrastive decoding 消除被放大的错误概念。

## 2. Method：Instruction Contrastive Decoding（ICD）

- 对同一图像和问题构造 standard instruction 与 disturbance instruction。
- 分别得到两个 next-token distribution：标准输入反映正常视觉理解，扰动输入暴露易被激活的 hallucinated concepts。
- 通过对比/相减两个分布，降低由 disturbance instruction 放大的 token 概率，保留真正由视觉证据支持的 token。
- 这是 inference-time 方法，不需要重新训练 LVLM。

## 3. Baseline 与实验设置

- **LVLM backbone**：InstructBLIP 与 miniGPT-4，底层使用 Vicuna-7B 和 Q-Former 等视觉语言融合结构。
- **LLaVA-Bench**：开放式生成 hallucination 评测，包含 24 张图像。
- **MSCOCO validation**：用于对象 hallucination、共现偏置和视觉 grounding 分析。
- 与 standard decoding、其他 hallucination mitigation/contrastive 方法对比，以 object hallucination rate 等指标评价。

## 4. Findings

- disturbance instruction 会把错误对象概念（论文示例中如 person、fork）推高；对比解码后，正确对象（如 dog）的概率恢复。
- ICD 在两个 LVLM backbone 上都能减少 object hallucination，说明方法利用的是推理分布差异，而非某个模型的训练参数。
- 方法对对象属性和关系也有帮助，但复杂关系需要更强的视觉证据，不能只靠 token-level subtraction 完全解决。
- 作为 inference-time 方法，ICD 避免了重新训练成本，但每个 token 需要额外计算 disturbance distribution，带来推理开销。

## 5. 对比与局限

必须区分“减少幻觉”和“变得更保守”：若模型只是少生成对象，hallucination rate 可能下降但 recall 也下降。因此应同时报告 object recall、回答完整性和事实性。disturbance instruction 的设计也会影响结果，错误 instruction 可能产生无意义的对照分布。

**一句话评价**：ICD 将 LVLM 的幻觉当作可被扰动 instruction 放大的分布成分，再通过对比解码去除，是一种不改参数但直接作用于视觉语言生成分布的解释性控制方法。
