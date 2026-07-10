# 127. Evaluating Object Hallucination in Large Vision-Language Models

> 人工精读笔记：EMNLP 2023。重点是 POPE 如何把开放 caption hallucination 转成稳定的 object existence polling。

## 0. 论文信息

- **作者**：Yifan Li, Yifan Du, Kun Zhou, Jinpeng Wang, Wayne Xin Zhao, Ji-Rong Wen
- **来源**：[EMNLP 2023](https://aclanthology.org/2023.emnlp-main.20)
- **方法**：POPE（Polling-based Object Probing Evaluation）
- **数据/模型**：MSCOCO；多种 LVLM

## 1. Introduction：要解决什么问题

LVLM 会描述图像中不存在的对象。传统 instruction-based caption evaluation 受生成长度、开放词汇和人工评价影响，CHAIR 主要在 caption/object annotations 上计算，难以灵活控制 negative object 难度。

## 2. Method / Framework：怎么解决

POPE 将 hallucination 转成 yes/no object existence questions，并从图像 annotation 采样 positive/negative objects。提供 random、popular、adversarial 三种 negative sampling：随机负对象、数据集中常见对象、与图像共现导致更易混淆的对象。

用 accuracy、precision、recall、F1 等回答指标测模型是否会把不存在对象说成存在，避免只依赖一段自由生成文本。

## 3. Baseline / 对比基线

CHAIR sentence/instance score、传统 caption instruction、不同 negative sampling、多个 LVLM/VLPM。POPE 的三个设置本身也是难度 baseline。

## 4. Experiments / Findings：结果如何读

初步实验显示许多 LVLM 的 object hallucination 很严重，甚至比小型视觉语言模型更容易生成高频共现对象。高频出现在 visual instruction 或与图像对象共现的对象更容易被 hallucinate，说明语言先验和视觉证据之间存在偏置。

POPE 的 random/popular/adversarial 设置比开放 caption 更稳定、可比较；adversarial setting 能揭示模型是否依赖 co-occurrence shortcut，而不是只看显著视觉 object。

## 5. Ablation / 机制解释

不同 negative sampling、prompt、模型、temperature/解码和 CHAIR 对照构成主要实验。结果说明评测协议本身会显著影响 hallucination rate，报告单一 POPE accuracy 也需要说明 sampling 方案。

## 6. Limitations / 局限与复现注意

POPE 主要测 object existence，不覆盖数量、属性、空间关系和复杂视觉推理；annotation 漏标会被当作负例；yes/no 格式可能比开放对话容易。

## 7. 两句话总结

POPE 将 LVLM 对象幻觉转成可控的 yes/no polling，并用 random/popular/adversarial negatives 稳定比较模型。实验表明语言 instruction 中的高频和共现对象会诱发幻觉，但 POPE 不覆盖更细粒度的属性与关系错误。
