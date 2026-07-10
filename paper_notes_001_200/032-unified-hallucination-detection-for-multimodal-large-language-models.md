# 032. Unified Hallucination Detection for Multimodal Large Language Models

- **作者 / venue**：Wenhao Yang, Yifei Wang, Yixuan Xie, Shilong Zhang, et al.；ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.178/)
- **任务**：统一检测 MLLM 的 object、attribute、scene-text 和 fact-conflicting hallucination

## 1. Introduction

MLLM 的 hallucination 不只是不认识图像中的对象，还包括属性错误、OCR/scene-text 错误、关系错误，以及 text-to-image 中与事实冲突的生成。已有 detector 往往只覆盖 image-to-text 的一个类型，导致不同任务之间不能比较。

## 2. Method / benchmark：UniHD 与 MHaluBench

- 统一 problem formulation，将图像-文本和文本-图像任务放进同一检测框架。
- 构建 MHaluBench，细粒度标注 modality-conflicting 和 fact-conflicting hallucination。
- 使用辅助工具验证候选答案/描述，例如 OCR、视觉检测、外部 fact checking 或 API 结果。
- 评估片段级和回答级风险，而不是只问整条 response 是否 hallucinated。

## 3. Baseline 与对比

- Self-Check 2-shot。
- Self-Check 0-shot。
- 不同 hallucination 类型和模态任务的统一比较。
- image-to-text 与 text-to-image 的对照，检验 detector 是否依赖单一生成方向。

## 4. Findings

- MHaluBench 显示，object hallucination、attribute hallucination、scene-text hallucination 和 factual conflict 的难度不同，不能合并成一个总分。
- UniHD 通过多种验证工具能更细粒度地定位冲突片段，较简单的 self-check 更适合诊断具体错误来源。
- text-to-image 的 fact conflict 与 image-to-text 的 visual grounding 错误需要不同验证器；统一接口不代表所有模态共享同一判定规则。

## 5. 局限

辅助工具本身可能有识别误差；外部 API 的知识和视觉覆盖会影响标签。细粒度 hallucination 评测成本高，跨模型、跨语言和开放世界图片仍需要更多人工验证。

**一句话评价**：论文贡献在于把 MLLM 幻觉从单一 object error 扩展成跨模态、跨层级的统一检测问题，并提供可复用 benchmark。
