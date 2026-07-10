# 234. Activation Patching for Interpretable Steering in Music Generation

- **Authors:** Simone Facchiano, Giorgio Strano, Donato Crisostomi, Irene Tallini, Tommaso Mencattini, Fabio Galasso, Emanuele Rodolà
- **Venue:** arXiv 2025
- **Paper:** https://arxiv.org/abs/2504.04479
- **Tags:** Music Generation, Activation Patching, Steering, Audio Interpretability

## Introduction

大音频模型能生成音乐，却难以解释 tempo、timbre、energy 等属性在哪些层表示；文本 prompt steering 也无法明确区分“模型知道什么”和“哪段 activation 造成输出”。论文把 activation patching 与 concept direction identification 用于 music generation，建立可解释、可控的音频属性干预。

## Method / Framework

从属性对比 prompt（如 energetic/upbeat vs calm）提取 activation directions，用 DiffMean 等方法得到层/位置上的 steering signal；再 patch 或注入方向生成音乐，逐层分析属性响应。评估同时看目标 attribute effectiveness 与 Fréchet Audio Distance，避免只提高属性却生成不可听音频。

## Baselines / Comparisons

比较 no steering、不同 direction identification、层位置和 steering strength，并以原始音乐生成模型/文本 prompt 控制作对照。指标包括 tempo/BPM、timbre spectral centroid 等属性变化和 FAD 音频质量。

## Experiments / Findings

- 四类属性上 steering 带来约 20%--40% relative improvement；tempo 约改变 30--50 BPM，timbre 的 spectral centroid 变化约 360--720 units。
- strength 增大时目标属性更强，但超过约 1.5 后 FAD 快速上升，生成音频出现量化/质量伪影；这直接展示了可解释控制与内容质量的 trade-off。
- layer-wise activation response 能识别哪些层更适合某一音乐属性，说明音乐模型的 residual representation 中存在可干预属性方向，但不一定是单一 neuron。

## Ablation / Error Analysis

消融 direction、layer、prompt pair、steering coefficient 和目标属性。方向估计受 prompt wording/音乐样本分布影响，strength 过大破坏连贯性，quantized codec artifacts 会掩盖属性提升；FAD 对人类听感也只是 proxy。

## Limitations

实验模型、音乐类型和属性有限，不能直接外推到长结构、歌词、多声部或商业生成器。activation patching 需要白盒访问，且属性 direction 可能纠缠；人类听觉研究和真实创作控制仍需补充。

## 两句话总结

论文用 activation direction/patching 把 music generation 的 tempo、timbre、energy 转成可读且可调的内部控制变量。属性能提升 20%--40%，但强 steering 会牺牲 FAD 与音质，说明可控性必须和生成质量共同优化。
