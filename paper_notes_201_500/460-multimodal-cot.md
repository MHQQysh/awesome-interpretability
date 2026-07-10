# 460. Multimodal Chain-of-Thought Reasoning in Language Models

- **Authors:** Zhuosheng Zhang, Aston Zhang, Mu Li, Hai Zhao, George Karypis, Alex Smola
- **Venue / year:** TMLR 2024
- **Paper:** [arXiv:2302.00923](https://arxiv.org/abs/2302.00923)
- **Tags:** `multimodal-reasoning` `chain-of-thought` `hallucination` `vision-language`

## Introduction
语言 CoT 的中间 rationale 在小模型上可能错误并反过来误导答案；在视觉问答中，如果只把图像转成 caption，模型仍可能缺失决定性视觉事实。论文提出 Multimodal-CoT，研究视觉信息是否能生成更可靠的 rationale，并让答案推理与 rationale 生成分离。

## Method / Framework
框架是两阶段：第一阶段把 question/context/options 与 ViT image features 融合，生成 rationale；第二阶段将原输入与 rationale 一起输入 decoder，预测答案。默认使用 FLAN-Alpaca 初始化的 T5 Base 223M 或 Large 738M，冻结 ViT-Large，分别微调两个阶段；不依赖超过 1B 参数的 teacher 才能运行。

## Baselines / Comparisons
比较 No-CoT 直接 `QCM→A`、one-stage `QCM→RA/AR`、无视觉的 two-stage、caption 输入、不同 backbone 和 vision feature。外部 baseline 包括 MCAN、Top-Down、BAN、DFAF、ViLT、Patch-TRM、VisualBERT、UnifiedQA、GPT-3.5/ChatGPT/GPT-4、LLaMA-Adapter、LLaVA、InstructBLIP，并在 ScienceQA/A-OKVQA 评估。

## Experiments / Findings
ScienceQA 的 text-only one-stage No-CoT 为 81.63%，生成 rationale 后再答只有 69.32%；two-stage baseline rationale Rouge-L 90.73、answer 78.57。加入 caption 只到 79.37，而注入 vision features 后 Rouge-L 93.46、answer 85.31。ScienceQA 上 Multimodal-CoT Large 为 90.45，超过当时 published best 86.54；A-OKVQA Base 为 50.57。

视觉特征在随机抽取的 hallucination errors 中修正了 60.7%；训练曲线显示两阶段多模态模型更早收敛。没有人工 rationale 时，用 InstructBLIP/ChatGPT 生成伪 rationale 的平均准确率 87.76，低于人工标注 90.45，但仍明显有用。MMMU zero-shot 上 738M Multimodal-CoT 为 28.7，超过若干更大模型。

## Ablation / Error Analysis
去掉 two-stage 后 Base/Large 为 82.62/84.56，去掉 vision features 为 78.57/83.97，完整模型为 85.31/90.45。ViT、CLIP、DETR、ResNet 特征准确率分别 85.31、84.27、83.16、82.86；不同 backbone 仍有收益。50 个错误样本中 80% 是 commonsense、14% logical、6% 是 rationale 为空/正确但答案错，提示计数、地图和常识仍是主要缺口。

## Limitations
效果依赖带 rationale 的 ScienceQA 监督和 ViT 特征，视觉-语言对齐仍较弱；两阶段不会自动使 rationale faithful，错误 rationale 仍可能影响答案。模型和数据规模小于通用 VLM，外部泛化和真实开放域图像需要更多验证。

## 两句话总结
Multimodal-CoT 用视觉特征生成更可靠的 rationale，再用第二阶段完成答案推理，解决小模型 CoT 会自我误导的问题。它在 ScienceQA 达到 90.45% 并降低视觉幻觉，但 commonsense/logical error 和 rationale faithfulness 仍限制实际可靠性。

## Evidence note
已读取 one-stage/two-stage 对照、vision-feature 注入、ScienceQA/A-OKVQA 主表、伪 rationale、收敛分析、特征 ablation 和 50 样本 error analysis。
