# 222. ViperGPT: Visual Inference via Python Execution for Reasoning

- **Authors:** Dídac Surís, Sachit Menon, Carl Vondrick
- **Venue:** ICCV 2023
- **Paper:** https://doi.org/10.1109/ICCV51070.2023.01092
- **Tags:** Visual Program Synthesis, Python Execution, Compositional Reasoning

## Introduction

端到端 VQA 把视觉感知和逻辑推理混在一个黑盒里，难以组合、调试和迁移；手写模块程序又需要为每个任务训练 program generator。ViperGPT 的问题是能否让 code-generation LLM 根据自然语言查询自动组合现成视觉、语言、数学和逻辑模块。

## Method / Framework

作者给 LLM 一个 API，描述 ImagePatch、VideoSegment 及 object detection、depth、caption、VQA、knowledge query 等模块；LLM 输出 Python function，系统编译执行，并把中间变量和函数调用作为可读推理路径。程序可以先检测/裁剪，再计数、比较、调用常识或做算术，解决单一视觉模型无法完成的 compositional query。

## Baselines / Comparisons

在 RefCOCO/RefCOCO+ referring expression、GQA、NLVRv2、CLEVR、VQA 和视频/知识任务上比较 supervised MDETR/OFA/ViLT 与 zero-shot OWL-ViT、GLIP、ReCLIP 等。指标是 grounding IoU、VQA accuracy、NLVR accuracy 和程序/中间结果的可解释性；还比较 curated/random/voting prompt。

## Experiments / Findings

- RefCOCO testA 上 ViperGPT zero-shot IoU 72.0，RefCOCO+ 67.0，高于 OWL-ViT 30.3/29.4、GLIP 55.0/52.2 和 ReCLIP 58.6/60.5，接近但低于 supervised MDETR/OFA。
- GQA 子集上 voting 达 50.5 accuracy，高于 ViLT-VQA 47.8；NLVRv2 不训练任务模块即可 62.4，说明程序组合能把现成能力连接起来。
- 中间 program 是可审计 rationale：错误可归因于检测、过滤、计数、知识查询或逻辑运算，而不是只能观察最终答案；模块升级后程序系统可继承新能力。

## Ablation / Error Analysis

消融 prompt example selection、multi-run voting 和模块类型；voting 缓解 LLM 程序随机性但增加调用成本。程序语法/参数错误、视觉模块漏检、类型不匹配、知识缺失和 LLM 选择了错误的分解策略都会造成级联失败。

## Limitations

ViperGPT 的“解释”是执行轨迹，不保证每个模块输出正确或与模型真实内部推理等价；依赖外部 API/模型，成本和安全边界复杂。复杂视频、长链、模糊图像和需要端到端 differentiable learning 的任务仍有限。

## 两句话总结

ViperGPT 用 LLM 生成可执行 Python，把检测、语言、知识和算术模块组合成无需训练的视觉推理程序。它带来强的组合能力和可调试轨迹，但程序质量最终受每个外部模块与代码生成错误的级联影响。
