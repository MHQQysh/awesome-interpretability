# 205. Rethinking Vision-Language Model in Face Forensics: Multi-Modal Interpretable Forged Face Detector

- **Authors:** Xiao Guo, Xiufeng Song, Yue Zhang, Xiaohong Liu, Xiaoming Liu
- **Venue:** CVPR 2025
- **Paper:** https://doi.org/10.1109/CVPR52734.2025.00019
- **Tags:** Deepfake Detection, CLIP, MLLM Explanation, Forgery Prompt

## Introduction

传统 deepfake detector 输出 fake probability，视觉问答式模型能生成解释却常常检测不准；真实部署需要同时回答“是真是假”和“哪里/为什么像伪造”。论文提出 M2F2-Det，把 CLIP 的跨模态泛化能力、专门的 forgery representation 和 LLM 的语言解释结合起来。

## Method / Framework

Forgery Prompt Learning 由 universal forgery prompts 与 layer-wise forgery tokens 组成：前者学习通用/特定伪造模式，后者在 CLIP text encoder 各层注入伪造语义。Bridge Adapter 把冻结或适配后的 CLIP image features 接到 EfficientNet-B4 detector 与 Vicuna-7B LLM；检测头输出 fake score，LLM 输出带视觉证据的自然语言 explanation，训练还对齐 forged feature 表示。

## Baselines / Comparisons

检测在 FaceForensics++、Celeb-DF、WildDeepfake、DFD、DFDC、FFIW 上比较 RFM、Freq-SCL、F3-Net、MultiAtt、RECCE、TALL 等；解释在 DD-VQA 上用 judgment accuracy/F1 以及 BLEU-4、CIDEr、ROUGE-L、METEOR、SPICE。还与只生成文字再读出 fake 的 DDVQA-BLIP 比较，并测试跨数据集泛化。

## Experiments / Findings

- Intra-dataset 表中 M2F2-Det 在 FF++ c23/c40、Celeb-DF、WDF 均达到强结果，例如 FF++ c40 accuracy/AUC 为 93.83/96.58，Celeb-DF 为 98.98/99.92，WDF 为 86.05/93.14。
- 与 TALL 相比，单帧 M2F2-Det 在 FF++ c40 accuracy 高 1.01 个百分点、AUC 高 2.01；在 Celeb-DF accuracy/AUC 高 1.41/1.37，说明 prompt-adapted CLIP 对 unseen forgery 仍有泛化。
- FPL 与 Bridge Adapter 都重要：FPL 的 layer-wise token 和 universal prompt 分别增加 FF++ AUC，组合效果更好；完整模型也明显超过 DDVQA-BLIP 的纯生成式检测。

## Ablation / Error Analysis

消融 UF-prompts、LF-tokens、Bridge Adapter 和不同训练阶段。只用基础 EfficientNet 缺少伪造域知识，去掉 LF 或 UF 会使 forged attention map 变粗；去掉 Bridge Adapter 会损失 CLIP 表示到 detector/LLM 的连接。错误包括极细微伪造、真实图像的异常光照、跨域压缩和 LLM 生成的合理化错误解释。

## Limitations

解释质量用 n-gram/embedding 指标与 DD-VQA 标注近似，不能保证 explanation 指向真正的伪造因果证据；LLM 的幻觉和训练数据偏差仍会传递到用户。跨视频时序、未知生成器、对抗伪造和真实平台分布的长期鲁棒性还需更多验证。

## 两句话总结

M2F2-Det 用 forgery prompt learning 学习伪造特征，再通过 Bridge Adapter 同时连接分类头和 LLM，令模型既判 fake/real 又给出文字证据。它在多种 deepfake 数据集上提升检测与跨域泛化，但自然语言解释仍不能替代可验证的局部伪造定位。
