# 055. Plausible May Not Be Faithful: Probing Object Hallucination in Vision-Language Pre-training

- **作者 / venue**：EACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.eacl-main.156/)

## Introduction 与方法

VLP image captioning 模型会生成不存在的对象；语言流畅和 CIDEr/BLEU 高并不能证明 caption 忠实于图像。论文系统研究 object hallucination 的来源、评测与缓解，区分视觉证据不足、语言先验和训练目标的影响。

## Baseline 与对比

比较 CLIP、VinVL 等 VLP models，在 COCO Caption（in-domain）和 NoCaps（out-of-domain）上测试。SCST 作为强化 caption metric 的 baseline：它能提高 CIDEr/BLEU，却可能增加 hallucinated objects。

## Findings

- 即使是强 VLP 模型，仍约有 **10%** 生成句子包含 hallucinated object；域外 NoCaps 更严重。
- 标准 caption quality metric 可能鼓励模型生成常见对象，因而与 object faithfulness 脱钩。
- SCST 提高 CIDEr 约 11.1、BLEU-4 约 2.7 的同时，增加 CHAIRs，说明优化表面指标会牺牲视觉忠实度。
- Object Masked Language Modeling（ObjMLM）通过 mask caption 中的 object，促使模型依赖图像对象对齐；object hallucination 最多降低约 **17.4%**。

## 局限

CHAIR 等指标依赖 object vocabulary 和 detector；图像中隐含/细粒度对象可能被误判。缓解 object hallucination 也可能减少 caption diversity。

**一句话评价**：论文最重要的警告是 plausible caption 不等于 faithful caption，并证明训练目标会直接决定模型更依赖语言先验还是视觉证据。
