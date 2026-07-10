# 359. ForgeryGPT: A Multimodal LLM for Interpretable Image Forgery Detection and Localization

- **Authors:** Fanrui Zhang, Jiawei Liu, Jiaying Zhu, Esther Sun, Dong Li, Qiang Zhang, Zheng-Jun Zha
- **Venue / year:** IEEE Transactions on Image Processing, 2026 arXiv revision
- **Paper:** https://arxiv.org/abs/2410.10238
- **Tags:** `multimodal-llm` `image-forgery` `localization` `explainable-vision`

## Introduction
Classical image-forgery detectors usually predict a score or mask from low-level texture/frequency artifacts, but they do not tell a user which object was forged, where it is, or why. Generic MLLMs such as LLaVA and GPT-4o can describe images, yet their web-scale pretraining does not provide the fine-grained forensic sensitivity needed for subtle tampering.

ForgeryGPT targets image-level detection, pixel-level localization, forgery type/object identification, and multi-turn explanation in one system. The paper's interpretability claim is operational: the model must produce a mask and a textual explanation that can be compared with ground-truth descriptions and used by people to revise their judgment.

## Method / Framework
The model has an image encoder, a Mask-Aware Forgery Extractor, and an LLM. The extractor contains a Forgery Localization Expert (FL-Expert) and Mask Encoder. FL-Expert uses an object-agnostic forgery prompt, a vocabulary-enhanced vision encoder, multi-scale features, and cross-modal reasoning to capture subtle traces. The Mask Encoder converts predicted masks into tokens aligned with the LLM feature space; learnable prompt tokens bridge image and mask modalities.

Training has three stages: image-text alignment on filtered CC3M, mask-text alignment on 76,013 synthetic forged image-caption pairs covering splicing, copy-move, and object removal, and task-specific instruction tuning on 48,000 dialogue instances from CASIA/Fantastic-Reality and related data. Vicuna-7B is the main LLM backbone; the final task supports detection, localization, and multi-turn questions about forged objects, location, and type.

## Baselines / Comparisons
Localization baselines include H-LSTM, HP-FCN, GSRNet, SPAN, SATL-Net, CAT-Net, PSCCNet, HiFi-Net, HiFi-Net++, ManTra-Net, MVSS-Net, and DiffForensics. The paper also compares recent LLM-oriented FAKESHIELD and SIDA under matched training conditions.

Image-level detection baselines use the same family, and explanation baselines are LLaVA and GPT-4o. Cross-domain anomaly localization is compared with AnomalyGPT on MVTec-AD and VisA. Pixel localization uses AUC/F1, detection uses accuracy/F1, and explanation uses CSS, BLEU-4, ROUGE-L, and SPICE.

## Experiments / Findings
Across eight localization datasets, ForgeryGPT has the highest reported average pixel performance, is best on CASIA1.0+, NIST16, IMD2020, DSO-1, and Korus, and is second on AutoSplice and Columbia. On IMD2020 it reaches 91.4% AUC. For image-level detection on CASIA1.0+, Columbia, IMD2020, and AutoSplice, its average accuracy/F1 is 0.804/0.791, above DiffForensics 0.770/0.776, FAKESHIELD 0.767/0.757, and SIDA 0.702/0.698.

For explanation accuracy, ForgeryGPT scores CSS 0.749, BLEU-4 0.240, ROUGE-L 0.284, and SPICE 0.309, with average 0.395, ahead of GPT-4o's 0.365 and LLaVA's 0.341. In the human study on 100 IMD2020 forged images, participants initially recognized 74% as fake; after reading explanations, 81% of initially missed fakes were corrected, and confidence increased for 65% of initially recognized fakes.

## Ablation / Error Analysis
Removing the whole extractor drops average detection accuracy/F1 to 0.680/0.669. Removing the prompt, Mask Encoder, or changing the LLM to Qwen/InternVL also lowers results; the full model is 0.804/0.791, compared with 0.778/0.772 without the prompt and 0.761/0.741 without the Mask Encoder. Localization ablations show that object-agnostic prompts, vocabulary enhancement, and multi-scale fusion all contribute to average F1/AUC.

Robustness tests apply resize, blur, noise, and JPEG compression on NIST16; ForgeryGPT remains ahead of the listed MVSS-Net, PSCCNet, and HiFi-Net baselines across distortions. The cross-domain MVTec/VisA results are competitive but below AnomalyGPT, showing that a forgery-specialized model does not automatically dominate generic anomaly localization.

## Limitations
The system depends on generated mask-text data and GPT-4o annotations, so synthetic artifacts and teacher wording can shape the learned explanation. Human confidence gains measure persuasion as well as correctness, and higher confidence is not itself proof of faithful localization. Performance varies with the MLLM backbone and pretraining data, and the paper does not establish robustness to every emerging diffusion manipulation.

## 两句话总结
ForgeryGPT 将伪造 mask 编码成 LLM 可理解的 token，并用 object-agnostic prompt、视觉词表和多尺度 FL-Expert 把像素级取证线索接入 Vicuna，从而同时做检测、定位和多轮解释。它在多个伪造数据集上超过传统方法，理由质量也优于 LLaVA/GPT-4o，但解释的说服力、合成数据依赖和跨域异常检测差距仍需谨慎解读。

## Evidence note
本笔记依据 arXiv v4 全文逐节核对；论文 PDF 标注为 IEEE TIP 2026 revision，表 II/III/V 与正文中的数值按原文抄录。
