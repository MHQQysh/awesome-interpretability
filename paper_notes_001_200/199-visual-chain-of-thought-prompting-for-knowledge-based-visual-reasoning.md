# 199. Visual Chain-of-Thought Prompting for Knowledge-Based Visual Reasoning

- **Authors:** Zhenfang Chen, Qinhong Zhou, Yikang Shen, Yining Hong, Zhiqing Sun, Dan Gutfreund, Chuang Gan
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/27888
- **Tags:** Visual CoT, Knowledge-Based VQA, Rationale, Multimodal Reasoning

## Introduction

知识型视觉推理不是只识别图中物体，还要把视觉概念与开放世界知识连接起来回答问题。已有方法常把视觉 perception、语言 reasoning 和 rationale 生成拆开，导致模型没有根据问题选择相关区域，最终答案也难以审计；VCTP 试图把这些步骤变成交互式推理轨迹。

## Method / Framework

VCTP 有 see、think、confirm 三阶段：see 用视觉模型扫描并 grounding 候选概念；think 让 LLM 根据问题选择关键概念，把区域转成自然语言描述并生成答案；confirm 再生成 supporting rationale，用跨模态 classifier 检查 rationale 是否与视觉内容一致，并迭代 think-confirm。最终保留选区、描述、答案和 rationale，因而比一次性 caption 更透明。

## Baselines / Comparisons

在 OK-VQA（14,055 image-question pairs）和 A-OKVQA 上比较 PICa、CoT、GPV-2 等 fully supervised 或 in-context 方法；使用 OPT-66B，并扩展到 Llama-2-70B、BLIP2-Codex。除 answer accuracy 外，评估 rationale 的 BLEU 与 CLIP sentence similarity、关键概念 Recall-1/Recall-2，并做 cost-aligned 多查询比较。

## Experiments / Findings

- 默认 VCTP 在 A-OKVQA/OK-VQA 上的表中结果为 46.41/44.62，相比 CoT 的 41.53/38.13 和 PICa 的 42.40/42.94 更好；更强模型组合 Llama-2/BLIP2-Codex 可达到 50.5/54.4 或 53.2/53.8。
- rationale 的 BLEU / sentence similarity 为 14.34 / 81.22，略高于 CoT 的 14.19 / 80.92；关键是它保留了每步选择的视觉概念和理由，而不是只看最终文字。
- 消融去掉 attend、rationale 或 CLIP verification 都下降；默认模型 QA accuracy 46.41，而 Oracle-A 选择最相关视觉概念可达 47.72，说明“概念选择”仍是主要瓶颈，verification 能过滤误导性 rationale。

## Ablation / Error Analysis

Random-A/G/R 分别替换视觉区域、global caption 和 rationale，均低于完整模型；w/o A 的下降最大，说明问题相关的视觉 grounding 是核心。错误来自检测器漏掉小目标、caption 丢失关系、LLM 引入常识但与图像冲突，以及 confirm classifier 自身判断错误；多轮迭代过多还增加成本。

## Limitations

系统依赖视觉检测/描述器、LLM 和 CLIP verification，多模型误差会串联；多查询成本高，且 rationale 与答案一致不等于其真正驱动答案。OK-VQA/A-OKVQA 的开放知识范围、英文设置和已有 rationale 也限制了对真实多模态对话的外推。

## 两句话总结

VCTP 把知识型视觉问答拆成 see-think-confirm，让模型先选相关视觉概念、再推理并验证 rationale，从而同时提升准确率和过程透明度。它的主要收益来自 query-relevant grounding，而非单纯延长 CoT；概念选择、外部知识和多模型成本仍是瓶颈。
