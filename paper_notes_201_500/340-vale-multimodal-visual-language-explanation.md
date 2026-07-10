# 340. VALE: A Multimodal Visual and Language Explanation Framework for Image Classifiers using eXplainable AI and Language Models

- **Authors:** P. N. Natarajan, Athira Nambiar
- **Venue / year:** arXiv 2024
- **Paper:** https://arxiv.org/abs/2408.12808
- **Tags:** `multimodal-xai` `SHAP` `SAM` `image-classification` `visual-explanation`

## Introduction
Image classifiers can be accurate while remaining difficult for a user to inspect. A heatmap indicates where a model looked, but a non-expert may still want a natural-language statement of what object or region supported the class. VALE combines model-agnostic visual attribution with segmentation and a vision-language model to produce both a visual region-of-interest and a text explanation.

The paper is a pilot framework rather than a new classifier. Its question is whether SHAP's influential coordinates can be turned into a useful prompt for SAM, and whether the segmented object can then be described by a pretrained VLM. The authors test both a generic ImageNet case and a domain-specific underwater SONAR case where the raw image is difficult for general captioning models.

## Method / Framework
VALE has four stages. An image classifier produces the predicted class. SHAP computes feature/pixel contributions and selects the coordinate with the highest contribution. That coordinate is used as a point prompt to Segment Anything (SAM), which returns candidate masks; the selected region is then passed to a pretrained vision-language model for an image-to-text explanation. For the SONAR case, the authors also use prompt engineering to tell the VLM to describe only the target object and the relevant sonar view.

For ImageNet, the classifier input is resized to 224x224. The paper compares VGG16, InceptionV3, and DenseNet121 and selects DenseNet121 for the framework because it reaches 92.3% top-5 accuracy with 8.1M parameters. SHAP uses a maximum evaluation-parameter setting; the highest-scoring coordinate is the magenta ROI point. SAM produces multiple masks, and the target region is used as the visual explanation input.

The SONAR dataset contains 753 ship images, 123 plane images, and 578 seafloor images. The authors train a DenseNet-style classifier, use synthetic augmentation for imbalance, and evaluate classification with a report including precision/recall/F1. Visual explanation is assessed with manually annotated samples using IoU, while language quality is assessed with BLEU.

## Baselines / Comparisons
The classifier comparison is VGG16, InceptionV3, and DenseNet121. The explanation comparison includes standard LIME/SHAP-style visual explanations and the prior LIME+SHAP+SAM approach cited by the paper. VALE's claimed distinction is not a higher classification accuracy; it is the addition of a language explanation after visual attribution and segmentation.

The text-generation comparison uses several pretrained VLM/LLM captioning models, including LLaVA, with prompt variants that do or do not reveal the predicted class. This tests whether the VLM can explain the object from the image or merely benefits from being told the answer.

## Experiments / Findings
For ImageNet, DenseNet121 is selected with 75.0% top-1 and 92.3% top-5 accuracy in the table. On a bald-eagle example, the SHAP map identifies a point on the relevant object and SAM generates an object mask. The SHAP hyperparameter ablation shows that very small maximum evaluation counts, such as 100 or 200, can focus on irrelevant regions, while 300, 500, and 1000 more reliably identify the object.

The captioning experiment shows a strong prompt effect. For the bald-eagle example, prompts without the class label have low BLEU, while supplying the true label improves the score. LLaVA is the strongest among the listed models in the reported table; the exact scores vary by prompt, with values such as 0.1171 for “Explain?” and 0.2627 for a prompt explicitly naming “eagle.” This means the language stage is partly an explanation of a known class, not a fully independent verification of the classifier.

For SONAR, the custom classifier reaches 99.32% validation accuracy after 14 epochs. The visual SHAP/SAM pipeline produces object masks for planes and ships, but the images are pixelated and the default VLM can hallucinate ordinary-camera details. Prompt engineering that asks the model to describe only the object and its sonar context improves text overlap. The paper reports BLEU examples of 0.0732 for a default plane prompt versus 0.1216 for a more targeted prompt, and a best ship-style score of 0.2695 for the custom prompt.

The qualitative results are useful in a different way from the numbers: the generated explanations mention sonar grazing angle, shadows, hull/body shape, and the distinction between object and seafloor. The framework therefore supplies a user-facing bridge from attribution coordinate to segmented object to domain-conditioned language.

## Ablation / Error Analysis
The SHAP evaluation-parameter ablation is the main component study. Too few evaluations yield unstable coordinates and masks that include background; larger settings are more focused but cost more computation. Prompt ablation in SONAR shows that a generic “Explain the object” instruction is insufficient for domain language, while a prompt that names the target class and asks the model to ignore background gives higher BLEU.

The paper also exposes a faithfulness risk: giving the predicted class to the VLM raises BLEU, but it makes the captioning stage conditional on the classifier label. SAM's three candidate masks can also select similar regions or the entire object, so a good-looking segmentation is not proof that SHAP found the true causal pixels.

## Limitations
The evaluation uses a small number of manually annotated examples for IoU and BLEU because annotation and computation are limited. BLEU measures reference-word overlap, not whether the text is causally supported by the classifier or the image. SONAR data are custom and imbalanced, and the high validation accuracy may not represent deployment conditions. VALE adds SHAP, SAM, and a VLM inference chain, so latency and error propagation are higher than for the base classifier.

## 两句话总结
VALE 把 SHAP 的重要坐标交给 SAM 做 ROI 分割，再把分割结果交给 VLM 生成自然语言，从而同时输出“看哪里”和“为什么”。实验说明在 ImageNet 和 SONAR 上这个链条能产生更可读的解释，但 prompt、SHAP 预算和分类标签泄漏会明显影响 BLEU，因此它更像实用的 post-hoc XAI 框架而不是严格的因果解释器。

## Evidence note
This note was written from the paper PDF (arXiv:2408.12808v1), including the four-stage VALE architecture, classifier table, SHAP/SAM ablations, ImageNet/SONAR experiments, prompt BLEU results, and comparison/limitations sections.
