# 208. X-VARS: Introducing Explainability in Football Refereeing with Multi-Modal Large Language Models

- **Authors:** Jan Held, Hani Itani, Anthony Cioppa, Silvio Giancola, Bernard Ghanem, Marc Van Droogenbroeck
- **Venue:** CVSports Workshop 2024
- **Paper:** https://doi.org/10.1109/CVPRW63382.2024.00332
- **Tags:** Video-Language Model, Explainable Decision, Sports, Human Study

## Introduction

足球裁判判断犯规和牌级不仅依赖动作，还依赖接触强度、位置、意图、比赛规则和是否破坏进攻；仅输出 foul label 无法解释主观决策。X-VARS 把足球裁判作为高难度测试场景，研究多模态大模型能否从视频给出规则一致、可审计的答案与理由。

## Method / Framework

作者构建 SoccerNet-XFoul：超过 10k 视频片段、22k video-question-answer triplets，由 70 多位有经验裁判标注四类问题。X-VARS 以 Video-ChatGPT 为基础，用 CLIP ViT-L/14 提取帧级和 patch hidden states，做时空池化后投影到 LLM；先微调 CLIP，再用 QLoRA 只更新约 1% 层，联合进行 foul/severity recognition 与 explanation generation。

## Baselines / Comparisons

数据集与 SoccerNet-MVFoul、Sports-QA 等体育视频资源比较；模型层面比较 ResNet、R(2+1)D、VARs-MViT、CLIP-L/14 和此前视频 foul classifier。任务指标包括 foul/type/severity accuracy 与 balanced accuracy，另有与人类裁判对 explanation quality 的 1--5 分人工研究。

## Experiments / Findings

- 多任务分类中，X-VARS 对 foul type 的 accuracy/balanced accuracy 为 0.62/0.35，超过单独 CLIP-L/14 的 0.52/0.35 及 VARs-MViT 的 0.43/0.34 等对照，说明 LLM 适配能利用规则与语境。
- 人工研究中 X-VARS explanation 平均 3.8 分，人类裁判 4.0 分；46% 片段中模型解释获得更高分，整体分布接近人类，支持“可读且规则相关”而非只看分类分数。
- 质性例子显示模型能解释拉拽、接触强度和牌级理由，但同一视频不同裁判存在主观分歧，这也是模型无法简单匹配单一 gold rationale 的真实难点。

## Ablation / Error Analysis

消融 CLIP fine-tuning、LLM instruction tuning、CLIP text predictions 和训练标签形式。模型可能只记住 foul/severity token 而不看 video tokens，因此作者检查视觉输入依赖；常见错误是动作遮挡、摄像机视角不完整、规则边界主观以及视频中不存在的动作被 hallucinate。

## Limitations

裁判解释具有主观和法律/规则语境，单一平均分不能证明事实正确；数据主要是足球犯规，不能外推到其他运动或高风险执法。视频采样只有 16 帧、模型仍可能幻觉，且真实比赛中的实时延迟、隐私和责任归属没有解决。

## 两句话总结

X-VARS 用足球规则问答数据和视频-语言微调，把 foul/severity 识别与“为什么”解释放进同一个多模态模型。它的解释质量接近裁判、分类也优于视觉对照，但主观标签、视频缺失证据和 hallucination 让它更像辅助系统而非自动裁判。
