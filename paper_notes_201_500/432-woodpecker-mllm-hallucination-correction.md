# 432. Woodpecker: Hallucination Correction for Multimodal Large Language Models

- **Authors:** Dongzhi Wang, Xueyang Li, Longxiang Zhang, and others
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2310.16045
- **Tags:** `MLLM` `hallucination` `training-free` `visual-grounding` `Woodpecker`

## Introduction
Woodpecker 关注 MLLM 已经生成回答之后的视觉幻觉。论文区分 object-level hallucination（说出了不存在的物体或错误数量/位置）和 attribute-level hallucination（颜色等属性错误），指出单纯微调虽然可以降低幻觉，却常以牺牲回答细节为代价。于是作者把纠错从模型训练阶段移到推理后阶段，目标是利用外部视觉专家检查原回答，并把可验证证据写回答案。

## Method / Framework
Woodpecker 是 training-free 的五阶段流水线。第一步从 MLLM 回答中提取 key concepts；第二步让 LLM 为每个概念生成验证问题，例如“图中有这个物体吗”“它是什么颜色”“它位于另一个物体哪一侧”；第三步调用 open-set object detector 验证存在性、数量和候选框，调用 VQA 模型验证颜色/位置等属性；第四步由 LLM 把视觉知识整理成具体 visual claims；第五步用这些 claims 和 bounding boxes 修改原回答，并在提到物体的位置插入框坐标，方便人工复核。

这种分工很重要：检测器擅长开放集物体和计数，VQA 擅长上下文属性，LLM 负责把回答拆成可验证问题和把证据组织成自然语言。对 POPE 中只有 Yes/No 的问题，Woodpecker 先把问题与答案拼成完整 claim，再进行纠错，避免后续语言模型无法理解孤立的“Yes”。

## Baselines / Comparisons
基础 MLLM 包括 mPLUG-Owl、LLaVA、MiniGPT-4 和 Otter；在 POPE 上使用 random、popular、adversarial 三种设置，在 MME 上同时测 object-level 的 existence/count/position 和 attribute-level 的 color，并用 LLaVA-QA90 做开放式描述评测。开放式回答另外由 GPT-4V 直接看图、原回答和修正回答，评分为 accuracy 与 detailedness。

## Experiments / Findings
在 POPE 上，Woodpecker 将 MiniGPT-4 的 accuracy 从 54.67% 提升到 85.33%，将 mPLUG-Owl 从 62.00% 提升到 86.33%，论文报告相对提升分别为 30.66% 和 24.33%。在 adversarial POPE 中，mPLUG-Owl 的 accuracy 从 56.33% 提升到 81%；在 MME 中，LLaVA 总分由 421.67 提升到 565.00，MiniGPT-4 由 280.00 提升到 528.33，mPLUG-Owl 由 300.00 提升到 555.00，Otter 由 448.33 提升到 571.67。

开放式 GPT-4V 评测也显示四个 MLLM 的 accuracy 和 detailedness 都上升，例如 MiniGPT-4 的 accuracy/detailedness 从 7.0/6.4 到 8.2/8.8。说明框坐标不仅帮助纠错，也让输出包含更多可检查的视觉细节；不过这不等于所有新增文字都由独立人工事实核验。

## Ablation / Error Analysis
作者构造一个总是回答“Yes”的 default 模型，以隔离纠错模块本身。只接 detector 主要改善 existence 和 count，只接 VQA 主要改善 color；两者合并的 full Woodpecker 最好，说明视觉专家有互补性。MME 的纠错行为进一步分为保留/纠正后的 accuracy、未修正错误的 omission 和错误改动正确答案的 mis-correction；full 方法 accuracy 为 79.2%，而 omission 与 mis-correction 保持较低。

主要失败来自 detector 漏检或开放集类别不匹配、VQA 对位置关系不可靠、LLM 将问题和短答案拼接得不自然，以及 bounding box 被语言模型误读。论文也指出 position split 的增益小于 existence/count/color，部分原因是 BLIP-2 的位置推理能力和 LLM 解释框坐标的能力有限。

## Limitations
系统依赖多个 off-the-shelf 模型和多次 API/推理调用，延迟与工程复杂度较高；检测器/VQA 的错误会被 LLM 继承。GPT-4V 评测样本有限且本身不是严格的金标准，视觉关系、细粒度属性和复杂场景仍可能无法被验证；方法是后处理，不会修复基础 MLLM 的视觉表征或知识问题。

## 两句话总结
Woodpecker 把 MLLM 的回答拆成物体和属性 claims，用检测器、VQA 与 LLM 组成可解释的视觉事实核验链，再把证据写回原答案。它不训练新模型，却在 POPE、MME 和开放式 GPT-4V 评测中显著降低 object/attribute hallucination，代价是额外推理成本和对视觉专家可靠性的依赖。

## Evidence note
已逐段读取本地 PDF，核对了五阶段框架、POPE/MME/LLaVA-QA90 设置、四个 MLLM 基线、GPT-4V 评测、detector/VQA 消融及 omission/mis-correction 分析。
