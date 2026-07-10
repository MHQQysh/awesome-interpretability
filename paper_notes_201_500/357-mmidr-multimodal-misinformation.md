# 357. MMIDR: Teaching Large Language Model to Interpret Multimodal Misinformation via Knowledge Distillation

- **Authors:** Longzheng Wang, Xiaohan Xu, Lei Zhang, Jiarui Lu, Yongxiu Xu, Hongbo Xu, Minghao Tang, Chuang Zhang
- **Venue / year:** 2024 manuscript / arXiv preprint
- **Paper:** https://arxiv.org/abs/2403.14171
- **Tags:** `multimodal-misinformation` `knowledge-distillation` `rationale-generation` `evidence-grounding`

## Introduction
Multimodal misinformation cannot be judged from text alone: the caption, image, OCR, comments, and retrieved evidence may support or contradict one another. Existing detectors often expose extracted features or a label but do not produce a fluent rationale. The paper asks whether proprietary LLMs can supply that rationale and whether the capability can be distilled into accessible open-source LLM/MLLM students.

The target is a three-way label, non-rumor, rumor, or unverified, together with an explanation of the evidence and the cross-modal inconsistency. This makes interpretability part of the detector output rather than a separate visualization.

## Method / Framework
MMIDR first converts retrieval-enhanced image-text posts into instruction-following examples. A visual-processing module produces OCR and image captions, while an evidence-retrieval module obtains textual and visual evidence. The processed post, evidence, and a task instruction are sent to GPT-3.5-turbo, which generates the label and rationale.

The generated teacher data form the MR2-style instruction dataset with 12,493 instances: 11,184 training and 1,309 test examples, distributed across non-rumor, rumor, and unverified labels. LoRA then fine-tunes Llama2-Chat-7B or MiniGPT-v2 while the base model stays frozen; the student is trained to reproduce the teacher's conditional language output while retaining the original visual input pathway.

## Baselines / Comparisons
The conventional detection baselines are BERT+ResNet, CAFE, which models cross-modal ambiguity, and COOLANT, which uses cross-modal contrastive learning. The zero-shot baselines are GPT-3.5-turbo and MiniGPT-v2. MMIDR is evaluated with Llama2 and MiniGPT-v2 student backbones, with variants removing evidence and/or rationales.

The comparison is deliberately two-dimensional: label performance and explanation quality. The paper also compares the proprietary teacher with distilled students on case studies, where the teacher identifies key contradictions more reliably and the students often give fluent but incomplete evidence summaries.

## Experiments / Findings
In zero-shot evaluation, GPT-3.5 reaches 48.89 accuracy and 41.56 F1, while MiniGPT-v2 reaches 53.72 and 52.52. Removing visual or textual evidence changes performance only slightly, but removing all evidence reduces GPT-3.5 to 36.75 accuracy and 32.94 F1. The result indicates that general LLM knowledge is insufficient for reliable multimodal rumor detection without task evidence.

After fine-tuning, BERT+ResNet obtains 75.40 accuracy/73.61 F1, CAFE 78.69/75.71, and COOLANT 82.97/79.56. MMIDR-Llama2 reaches 93.63/93.62, and its no-rationale variant reaches 95.83/95.83; MMIDR-MiniGPT-v2 reaches 94.04/94.00, and its no-rationale variant reaches 95.95/95.92. Thus the distilled models are strong classifiers, while the full rationale objective trades a small amount of label score for the desired explanatory output.

The two detailed cases show the qualitative gap. GPT-3.5 points to a comment saying the news is fake or to conflicting India-China narratives; student models detect the broad lack of support but sometimes focus on irrelevant OCR or omit the decisive contradiction. This supports the claim that distillation transfers fluent explanation ability, but not all of the teacher's evidence prioritization.

## Ablation / Error Analysis
Removing rationale from the training/evaluation instruction gives the best classification score, because the task becomes simpler three-way classification. Removing evidence as well lowers Llama2 from 95.83 to 95.57 and MiniGPT-v2 from 95.95 to 94.19, confirming that evidence helps detection. The main MMIDR models are lower, 93.63 and 94.04, because the distillation objective emphasizes aligning explanatory generations and omits a dedicated task-classification loss.

The evidence-count analysis varies retrieved visual and textual evidence from 1 to 10. Accuracy peaks when the combined evidence count is about six but does not exceed MiniGPT-v2's 53.72 zero-shot score, and the fluctuation shows that too much evidence can overload the teacher. Human-facing rationales can also be factually fluent yet cite the wrong evidence, a failure visible in the student case study.

## Limitations
The dataset is built with proprietary GPT-3.5 rationales, so teacher errors and stylistic biases can be inherited. The distillation objective is not jointly optimized for classification and explanation faithfulness, and the paper acknowledges that students miss crucial evidence. The venue metadata in the manuscript is still a conference-placeholder template, and the evaluation covers one retrieval-enhanced dataset rather than broad social-media domains.

## 两句话总结
MMIDR 把图像 OCR、图像描述和外部证据整理成指令，让 GPT-3.5 生成多模态谣言标签与理由，再用 LoRA 将这种解释能力蒸馏到 Llama2 和 MiniGPT-v2。蒸馏模型的分类准确率远超传统 BERT+ResNet/CAFE/COOLANT，但完整理由训练会牺牲少量分类分数，学生模型也常常抓不到教师指出的关键矛盾。

## Evidence note
本笔记依据 arXiv v3 全文逐节核对；表 3/表 4 的数值按论文原表抄录，论文自身的 ACM venue 字段仍是 rights-confirmation 占位符。
