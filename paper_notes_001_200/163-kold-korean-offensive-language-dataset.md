# 163. KOLD: Korean Offensive Language Dataset

- **Authors:** Younghoon Jeong, Juhyun Oh, Jongwon Lee, Jaimeen Ahn, Jihyung Moon, Sungjoon Park, Alice Oh
- **Venue:** EMNLP 2022
- **Paper:** https://aclanthology.org/2022.emnlp-main.744
- **Tags:** Korean NLP, Offensive Language, Span Annotation, Dataset

## Introduction

offensive-language detection 的 hierarchy、target type、target span 和解释性研究主要集中在英文，直接迁移会受到韩语形态、文化语境和攻击对象分布差异影响。KOLD 提供韩语平台数据和上下文，填补可解释、分层标注资源缺口。

## Method / Framework

从 NAVER news 与 YouTube 收集 40,429 条评论，并把标题 / 视频标题作为上下文。标注分三层：Level A 是否 offensive；Level B target type（group、individual、others、untargeted）；Level C target group，并额外标注支持判断的 offensive span 和 target span。

用这些标注训练 Korean BERT / RoBERTa，分别做 offensiveness、target classification、target group 和 span detection，研究上下文是否帮助分类。

## Baselines / Comparisons

比较不带 title 与带 title 的 BERT / RoBERTa，层级任务与 flat classification，span extraction 与 sentence classification，并参照 OLID 等英文 offensive dataset。指标包括 F1、accuracy 和 span detection scores。

## Experiments / Findings

- KOLD 使 Korean BERT / RoBERTa 能有效完成 offensiveness 和 target classification，但 target group 与 offensive span 仍有较大提升空间。
- 加标题上下文后，offensiveness、target classification、target group classification 分别提升约 0.3、1.5、13.1 个点，说明语境对韩语攻击判断尤其重要。
- target group 分布与英文数据集差异明显，不能用英文标签比例假设韩语社会语境。
- span 标注让模型输出可审计证据，但短词、隐含讽刺、跨句语境和多重 target 仍容易漏检。

## Ablation / Error Analysis

主要消融上下文、模型架构、层级标签和 span supervision。错误集中于文化特定表达、反讽、指代不明和 target group 边界；只做 binary offensive classification 会隐藏这些差异。数据分布不平衡也会使 group / span 任务的宏平均表现低于 Level A。

## Limitations

平台样本和社会事件会造成时间、主题与用户群体偏差；offensive label 含有文化和标注主观性。数据含有令人不适的内容，不能直接当作所有韩语网络语言。模型结果还不是 LLM 的机制解释，而是可解释分类 benchmark。

## 两句话总结

KOLD 把韩语 offensive detection 扩展到上下文、攻击对象和证据 span 的分层标注，提供了 40,429 条可训练样本。实验显示标题上下文尤其改善 target-group 识别，但文化语境、讽刺和不平衡仍是可解释检测的难点。
