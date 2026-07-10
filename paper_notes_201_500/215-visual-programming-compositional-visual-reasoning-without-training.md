# 215. Visual Programming: Compositional Visual Reasoning without Training

- **Authors:** Tanmay Gupta, Aniruddha Kembhavi
- **Venue:** CVPR 2023
- **Paper:** https://doi.org/10.1109/CVPR52729.2023.01436
- **Tags:** Neuro-Symbolic, Visual Programming, Compositional Reasoning, Rationale

## Introduction

复杂视觉任务往往要组合检测、计数、比较、知识检索和图像编辑，端到端模型难以训练且推理过程不可检查。VISPROG 的问题是能否不为每个任务训练新模块，而让 LLM 根据自然语言指令生成可执行的 modular program，并把每步中间结果变成人可读 visual rationale。

## Method / Framework

GPT-3 通过 in-context examples 生成 Python-like program；程序调用现成 vision models、image-processing routines、Python arithmetic/logical functions。每一行产生中间对象/图像/文本，后续模块消费这些结果，系统再将程序和中间输出总结为答案依据，因此“代码路径”同时是计算图和解释。

## Baselines / Comparisons

任务覆盖 compositional GQA、零样本图像对推理 NLVRv2、factual knowledge object tagging、language-guided image editing。与 ViLT-VQA、ViLT-NLVR 上限及不同 prompt strategy 比较，并测试 curated/random/voting in-context examples、原始/修改后 instruction 的准确率、precision、recall、F1。

## Experiments / Findings

- GQA 子集上 VISPROG voting 达 50.5 accuracy，高于 ViLT-VQA 的 47.8；NLVRv2 无训练达到 62.4，低于 fine-tuned ViLT-NLVR 的 76.3 但证明了零样本组合推理可行。
- knowledge tagging 从原始 instruction 的 63.7 F1 提升到检查 rationale 后修改 instruction 的 75.7；image editing 人工语义正确率从 59.8 提升到 66.4。
- 程序逐行输出让错误可定位：是检测、过滤、计数还是知识检索出错，而不是只看到一个最终答案；多次 voting 能缓解 GPT-3 程序生成的随机性。

## Ablation / Error Analysis

比较 curated/random/voting prompt、模块组合和修改 instruction；random 示例可用但 curated 不一定总能覆盖新组合，voting 增加调用成本却提高稳定性。错误来自程序语法/参数、现成视觉模块漏检、模块间类型不匹配、知识检索错误和 LLM 选择了不必要步骤。

## Limitations

系统能力上限仍由 off-the-shelf vision modules 和 GPT-3 代码生成决定，程序可执行不代表其 rationale 忠实或语义正确。没有 task-specific training 的优势伴随较高 prompt/执行成本，且复杂视频、细粒度关系和现实环境鲁棒性未充分覆盖。

## 两句话总结

VISPROG 把 LLM 的语言计划转成可执行视觉程序，用模块化中间结果同时完成 compositional reasoning 和可读解释。它在多个任务上展示零样本组合能力，但程序、视觉模块和知识检索的逐级错误仍决定最终可靠性。
