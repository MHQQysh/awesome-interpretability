# 416. HAGRID: A Human-LLM Collaborative Dataset for Generative Information-Seeking with Attribution

- **Authors:** Ehsan Kamalloo, Aref Jafari, Xinyu Zhang, Nandan Thakur, Jimmy Lin
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2307.16883
- **Tags:** `attribution` `citation` `human-LLM` `dataset` `information-seeking`

## Introduction
生成式搜索系统不仅要回答问题，还要给出可核验的证据和引用。用户的问题可能需要多跳检索、拒答或指出知识缺口；已有 QA 数据集往往只标一个答案，无法评价回答中的每句话是否被证据支持。HAGRID 构建人类与 LLM 协作的 attribution 数据集。

## Method / Framework
数据集让 annotators/LLM 在 information-seeking 对话中产生问题、答案、检索证据和 sentence-level attribution。每个生成句子可以对应 WikiData/知识图谱子图，也可以标记 `[NA]` 表示知识图谱中不存在、需要承认知识缺口的内容。BioKaLMA/biographical domain 用自动构造 pipeline 从 WikiData 生成 general/specific questions 和 minimum knowledge sets。

## Baselines / Comparisons
对比有 attribution 的生成、无 citation 的 vanilla answer、不同 retrieval/evidence selection 及 human/LLM annotator。一条回答的评价同时看内容质量、evidence coverage、citation correctness、knowledge gap awareness 和多跳支持，而不是只看 EM/F1。

## Experiments / Findings
HAGRID 的主要贡献是把“回答正确”“引用存在”“引用支持这句话”“知识是否缺失”拆开，使 attribution failure 可定位。数据展示了 human-LLM collaboration 能提高规模，但 LLM 可能生成没有证据的句子，必须保留 `[NA]` 这类 conscious incompetence 标签。

## Ablation / Error Analysis
问题类型、知识集大小、句子粒度和是否允许 `[NA]` 是主要对比。错误包括引用覆盖不完整、证据只支持句子一部分、知识图谱缺边被误判为 hallucination、多句 answer 共用一个 citation 造成错配。

## Limitations
知识图谱不是现实世界全部真值，自动构造可能带有 schema bias；biographical domain 不能代表所有开放检索。人工 attribution 昂贵，LLM annotator 的判断也要校验。数据集标注的是外部 evidence faithfulness，不是模型内部推理机制。

## 两句话总结
HAGRID 将生成式信息检索中的答案、证据、逐句 attribution 和知识缺口放入同一 human-LLM collaborative dataset，使引用忠实性可独立评估。它把“有 citation”推进到“citation 真正支持这句话”，但知识图谱覆盖和自动标注误差仍是关键边界。

## Evidence note
已读取 arXiv 2307.16883 本地 PDF 的 HAGRID/BioKaLMA 构造、minimum knowledge set、[NA] 语义和 attribution 目标。
